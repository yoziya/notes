---
title: Fast inverse square root
date: 2023-11-23
tags: [Algorithm]
categories: [GameDevelop]
---

# Fast inverse square root

快速平方根算法，是一种估计 $$\frac{1}{\sqrt{x}}$$ 的方法，这一算法的优势在于减少了求平方根倒数时浮点运算操作带来的巨大的计算开销。求两点之间距离经常需要求平方根倒数，因此广泛应用于计算机图形学领域。

该算法最著名的应用为[约翰·卡马克](https://zh.wikipedia.org/wiki/約翰·卡馬克)于1999年在《[Quake III Arena](https://en.wikipedia.org/wiki/Quake_III_Arena)》的源代码，其代码如下：

```cpp
float q_rsqrt(float number)
{
  long i;
  float x2, y;
  const float threehalfs = 1.5F;

  x2 = number * 0.5F;
  y  = number;
  i  = * ( long * ) &y;                       // evil floating point bit level hacking
  i  = 0x5f3759df - ( i >> 1 );               // what the fuck?
  y  = * ( float * ) &i;
  y  = y * ( threehalfs - ( x2 * y * y ) );   // 1st iteration
  // y  = y * ( threehalfs - ( x2 * y * y ) );   // 2nd iteration, this can be removed

  return y;
}
```

以上代码中起关键作用的代码如下：

- `i  = * ( long * ) &y;` ：这是在将浮点数转换为 long 整型。
- `i  = 0x5f3759df - ( i >> 1 );` ：通过一番魔鬼操作，得到 $\frac{1}{\sqrt{x}}$ 近似解，下面会讲到。
- `y  = * ( float * ) &i;` ：这是在将整型转换回浮点型。
- `y  = y * ( threehalfs - ( x2 * y * y ) );` ：用牛顿法迭代更精确的结果，上面代码中只用了一次牛顿法就得到了符合 float 类型精度要求的结果，因此注释掉了后面的代码。

## Algorithm

### Floating-point representation

要理解这个算法，首先要了解 **IEEE 754** 规范下的 32位（单精度）浮点型存储方式。

- 符号位（sign bit）：0 表示正数，1 表示负数，通常在最高位。科学计数法存储，整数位永远为1。
- 指数位（exponent）：有8位（double float 为11位），可以为值 0~255（$2^{8} - 1$），值127表示指数为0，因此能表示 $2^{-126}$ 到 $2^{127}$。
- 尾数位（mantissa）：存储数值的实际有效数字，有 23位，尾数用来表示一个介于 1~2（不包括 2） 之间的数，但最高位（隐含的”1”）通常不存储，因为除了在特殊情况（如0、无穷大、NaN）外，它总是1。

![image-20231203021800923](https://raw.githubusercontent.com/yoziya/images/main/notes/image-20231203021800923.png)

该浮点数的值计算公式如下：
$$
x = (-1)^{Si} \cdot (1 + \frac{M}{L}) \cdot 2^{(E - B)}
$$

其中常数 $L = 2^{23}$ （即尾数 23 位），$B = 127$（即为了能表示 $2^{-126}$ 到 $2^{127}$ 所需要的偏差值）。

对于图中的浮点数，符号位 $Si = 0_2 = 0$，指数位$E = 0111\ 1100_2 = 124$，尾数位 $M = 010\ 0000\ 0000\ 0000\ 0000\ 0000_2 = 0.25 \times 2^{23}$

带入公式得当前浮点数 $x =  1 \cdot (1 + 0.25) \cdot 2^{-3} = 0.15625$。

对于有符号整型，表示方法就是一个符号位和数值大小了。

这里，我们可以提前推导一下这个 float 类型视为有符号的整型后的值，下文需要用到。只需要把浮点型的指数位和尾数位再合并为一个整体，符号位不变，得如下结果。
$$
I_x = (-1)^{Si} \cdot (E \times L + M)
$$

### Aliasing to an integer as an approximate logarithm

对于想求 $\frac{1}{\sqrt{x}}$ 的 x，可以认为 x 中 $Si = 0$ 恒成立，再对 $x = (1 + \frac{M}{L}) \cdot 2^{(E - B)}$ 取对数，得到

​    $\log_{2}{x} = E - B + \log_{2}{(1 + \frac{M}{L})}$

然后，由于 $\frac{M}{L} \in [0,1)$

​    $\log_{2}{(1 + \frac{M}{L})} \approx \frac{M}{L} + \sigma$

即

​    $\log_{2}{x} \approx E - B + \frac{M}{L} + \sigma$

这时会发现转换为 int 后有近似值 $I_x = (E \times L + M) = L(E + \frac{M}{L}) = L(B + \log_{2}{x} - \sigma)$，那也就是对于 float 类型的 x，有

​    $\log_{2}{x} \approx \frac{I_x}{L} - B + \sigma$

那，如果我们跳过直接求 $y = \frac{1}{\sqrt{x}}$，改成求 y 被视为 Int 类型后的近似表示 $I_y$，可以得到
$$
\begin{align*}
y = \frac{1}{\sqrt{x}}\\
\log_{2}{y} = -\frac{1}{2}\log_{2}{x}\\
\frac{I_y}{L} - B + \sigma \approx -\frac{1}{2}(\frac{I_x}{L} - B + \sigma)\\
I_y \approx \frac{3}{2}L(B - \sigma) - \frac{1}{2}I_x
\end{align*}
$$
对应的就是

```
i  = 0x5f3759df - ( i >> 1 );
```

再讨论一下 $\sigma$，上面代码中实际 $\sigma \approx 0.0450466$，如果为 0，易得$\frac{3}{2}L(B - \sigma) = 0x5f400000$，$\sigma$ 的取值关乎 float 被视为 int 后 $I_x$ 与 $L(B + \log_{2}{x} - \sigma)$ 实际的接近程度，如果基于数学计算， $\sigma$ 理论最优值为
$$
\sigma = \frac{1}{2} − \frac{1 +\ln(\ln(2))}{2\ln(2)} \approx 0.0430357
$$

### Newton's method

牛顿法可以在有根的近似解的情况下提高精度。要求要有第一个近似值，且附近连续可导。

求 $y = \frac{1}{\sqrt{x}}$ 可以视为求 $f(y) = \frac{1}{y^2} - x$ 的根。用上面方法找到第一个近似值，对于近似值 $y_n$ 总能找到更好的近似值 $y_{n + 1} = y_n - \frac{f(y_n)}{f'(y_n)}$，即

$$
y_{n + 1} = y_n(\frac{3 - xy^2_n)}{2})
$$
对应的就是

```
y  = y * ( threehalfs - ( x2 * y * y ) );   // 1st iteration
```

### Additional

了解算法即可，硬件上的优化以至于它现在没什么用了。

---

## issues

### 



## reference

- [Fast inverse square root - Wikipedia](https://en.wikipedia.org/wiki/Fast_inverse_square_root)
- [Fast Inverse Square Root — A Quake III Algorithm - YouTube](https://www.youtube.com/watch?v=p8u_k2LIZyo)

## appendix

- 

---