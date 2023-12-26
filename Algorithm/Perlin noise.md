---
title: 柏林噪声
date: 2023-11-29
---



# note title

自然界中大部分噪声都是连续的，简单的使用随机数来实现白噪声往往效果不好，因此诞生了一堆噪声算法，最出名的就是 Perlin noise。

## Value noise

### example

```python
import numpy as np
import matplotlib.pyplot as plt

def lerp(a, b, t):
    """线性插值函数"""
    return a + t * (b - a)

def generate_noise_grid(width, height):
    """生成噪声网格"""
    return np.random.rand(width, height)

def value_noise(x, y, grid):
    """计算给定点的值噪声"""
    x0 = int(np.floor(x))
    x1 = x0 + 1
    y0 = int(np.floor(y))
    y1 = y0 + 1

    x0 = x0 % grid.shape[0]
    x1 = x1 % grid.shape[0]
    y0 = y0 % grid.shape[1]
    y1 = y1 % grid.shape[1]

    v00 = grid[x0, y0]
    v10 = grid[x1, y0]
    v01 = grid[x0, y1]
    v11 = grid[x1, y1]

    sx = x - x0
    sy = y - y0
    nx0 = lerp(v00, v10, sx)
    nx1 = lerp(v01, v11, sx)
    nxy = lerp(nx0, nx1, sy)

    return nxy

def create_noise_image(width, height, scale=1):
    """创建噪声图像"""
    grid = generate_noise_grid(width, height)
    image = np.zeros((height, width))

    for y in range(height):
        for x in range(width):
            image[y, x] = value_noise(x / scale, y / scale, grid)

    return image

# 图像尺寸和缩放
img_width, img_height, img_scale = 100, 100, 10

# 生成图像
noise_image = create_noise_image(img_width, img_height, img_scale)

# 显示图像
plt.imshow(noise_image, cmap='gray')
plt.colorbar()
plt.show()

```

### generate noise grid

要基于随机数生成连续的噪声，值噪声首先基于随机数生成一个网格结构，存为二维数组。

![image-20231203022811535](https://raw.githubusercontent.com/yoziya/images/main/notes/image-20231203022811535.png)

### value return

如果要取出u2.3，v1.6位置的点p的值

![image-20231203022941460](https://raw.githubusercontent.com/yoziya/images/main/notes/image-20231203022941460.png)

根据p值的坐标，对当前晶格的u 2~3求一个差值A，再对v1~2求一个差值B,最后在AB上求一个差值P。

如果直接使用线性插值
$$
\begin{aligned}
A = 0.11 * 0.7 + 0.57 * 0.3 = 0.248\\
B = 0.38 * 0.7 + 0.83 * 0.3 = 0.515\\
P = 0.248 * 0.4 + 0.515 * 0.6 = 0.4082\\
\end{aligned}
$$
最终得到的图像像这样

![image-20231203023048859](https://raw.githubusercontent.com/yoziya/images/main/notes/image-20231203023048859.png)

套用后来的柏林噪声提出的缓和曲线 $y = 6x^5 - 15x^4 + 10x^3$，结果如下

![image-20231203023103092](https://raw.githubusercontent.com/yoziya/images/main/notes/image-20231203023103092.png)

### generate gradient

![image-20231203023223040](https://raw.githubusercontent.com/yoziya/images/main/notes/image-20231203023223040.png)

### value return

![image-20231203023241509](https://raw.githubusercontent.com/yoziya/images/main/notes/image-20231203023241509.png)

和之前一样的三次插值，只不过晶格顶点值由完全随机改成了向量点积，如 $\vec{a_0} \cdot \vec{P_0P}$

![image-20231203023300263](https://raw.githubusercontent.com/yoziya/images/main/notes/image-20231203023300263.png)

### smoothstep

smoothstep函数的基本要求，x = 0 时 S(x) = 0，x = 1 时 S(x) = 1，(0,1)之间连续，高阶方程满足在两个端点处高阶导数为 0。如 $y = 6x^5 - 15x^4 + 10x^3$ 两端的一阶导数和二阶导数均为 0。

## Simplex noise

随着维度增加，柏林噪声需要越来越大的计算开销，Simplex噪声主要被用于改进改问题。

### example

```python
import numpy as np

def simplex_noise(xin, yin, perm):
    """计算2D Simplex噪声"""
    s = (xin + yin) * 0.5 * (np.sqrt(3.0) - 1.0)
    i = np.floor(xin + s)
    j = np.floor(yin + s)
    t = (i + j) * (3.0 - np.sqrt(3.0)) / 6.0
    x0 = xin - i + t
    y0 = yin - j + t

    i1, j1 = (1, 0) if x0 > y0 else (0, 1)

    x1 = x0 - i1 + (3.0 - np.sqrt(3.0)) / 6.0
    y1 = y0 - j1 + (3.0 - np.sqrt(3.0)) / 6.0
    x2 = x0 - 1.0 + 2.0 * (3.0 - np.sqrt(3.0)) / 6.0
    y2 = y0 - 1.0 + 2.0 * (3.0 - np.sqrt(3.0)) / 6.0

    ii = int(i) & 255
    jj = int(j) & 255
    gi0 = perm[ii + perm[jj]] % 12
    gi1 = perm[ii + i1 + perm[jj + j1]] % 12
    gi2 = perm[ii + 1 + perm[jj + 1]] % 12

    n0 = n1 = n2 = 0.0
    t0 = 0.5 - x0 * x0 - y0 * y0
    if t0 >= 0:
        t0 *= t0
        n0 = t0 * t0 * dot(grad3[gi0], x0, y0)

    t1 = 0.5 - x1 * x1 - y1 * y1
    if t1 >= 0:
        t1 *= t1
        n1 = t1 * t1 * dot(grad3[gi1], x1, y1)

    t2 = 0.5 - x2 * x2 - y2 * y2
    if t2 >= 0:
        t2 *= t2
        n2 = t2 * t2 * dot(grad3[gi2], x2, y2)

    return 70.0 * (n0 + n1 + n2)

def dot(g, x, y):
    """点积"""
    return g[0] * x + g[1] * y

# 梯度表和排列表
grad3 = np.array([[1,1,0],[-1,1,0],[1,-1,0],[-1,-1,0], 
                  [1,0,1],[-1,0,1],[1,0,-1],[-1,0,-1], 
                  [0,1,1],[0,-1,1],[0,1,-1],[0,-1,-1]])
perm = np.arange(256)
np.random.shuffle(perm)
perm = np.stack([perm, perm]).flatten()

# 创建Simplex噪声图像
def create_simplex_noise_image(width, height, scale=1):
    """创建Simplex噪声图像"""
    image = np.zeros((height, width))

    for y in range(height):
        for x in range(width):
            noise_val = simplex_noise(x / scale, y / scale, perm)
            image[y, x] = noise_val

    return image

# 生成图像
img_width, img_height, img_scale = 100, 100, 10
simplex_image = create_simplex_noise_image(img_width, img_height, img_scale)

# 显示图像
import matplotlib.pyplot as plt
plt.imshow(simplex_image, cmap='gray')
plt.colorbar()
plt.show()

```

固定了随机的梯度向量数组，将正方形晶格改为了三角形，减少了顶点数量

![image-20231203023326475](https://raw.githubusercontent.com/yoziya/images/main/notes/image-20231203023326475.png)



---

## issues



## reference

- [如何生成一张 Value Noise 算法图片 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/52054806)
- [Perlin noise - Wikipedia](https://en.wikipedia.org/wiki/Perlin_noise)
- [Smoothstep - Wikipedia](https://en.wikipedia.org/wiki/Smoothstep)

## appendix



---