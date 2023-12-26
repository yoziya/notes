---
title: 开源许可证
date: 2023-12-26
---



# License

大概 1980 前后，software 也拥有了自己的 copyright（版权）。此时一些人（[Richard Stallman](https://en.wikipedia.org/wiki/Richard_Stallman)）不满这种阻碍创新的制度，成立了 Free Software Foundation 和设计 [copyleft](https://en.wikipedia.org/wiki/Copyleft)（一种 License）。

现在更常用的 License 类型是 [permissive](https://en.wikipedia.org/wiki/Permissive_software_license)，由学界定义的标准。

## Comparison table

|                      | [Public domain](https://en.wikipedia.org/wiki/Public_domain) & [equivalents](https://en.wikipedia.org/wiki/Public-domain-equivalent_license) | [Permissive license](https://en.wikipedia.org/wiki/Permissive_license) | [Copyleft](https://en.wikipedia.org/wiki/Copyleft) (protective license) | [Noncommercial](https://en.wikipedia.org/wiki/Non-commercial_activity) license | [Proprietary license](https://en.wikipedia.org/wiki/Proprietary_license) | [Trade secret](https://en.wikipedia.org/wiki/Trade_secret) |
| :------------------: | :----------------------------------------------------------: | :----------------------------------------------------------: | :----------------------------------------------------------: | :----------------------------------------------------------: | :----------------------------------------------------------: | :--------------------------------------------------------: |
|     Description      |                      Grants all rights                       | Grants use rights, forbids almost nothing (allows proprietization, [license compatibility](https://en.wikipedia.org/wiki/License_compatibility)) | Grants use rights, forbids [proprietization](https://en.wikipedia.org/wiki/Proprietization) | Grants rights for noncommercial use only. May be combined with copyleft. | Traditional use of [copyright](https://en.wikipedia.org/wiki/Copyright); no rights need be granted |                 No information made public                 |
|       Software       |         PD, [CC0](https://en.wikipedia.org/wiki/CC0)         | [BSD](https://en.wikipedia.org/wiki/BSD_licenses), [MIT](https://en.wikipedia.org/wiki/MIT_License), [Apache](https://en.wikipedia.org/wiki/Apache_License) | [GPL](https://en.wikipedia.org/wiki/GPL), [AGPL](https://en.wikipedia.org/wiki/GNU_Affero_General_Public_License) | [JRL](https://en.wikipedia.org/wiki/Java_Research_License), [AFPL](https://en.wikipedia.org/wiki/Aladdin_Free_Public_License) | [proprietary software](https://en.wikipedia.org/wiki/Proprietary_software), no public license |                 private, internal software                 |
| Other creative works |         PD, [CC0](https://en.wikipedia.org/wiki/CC0)         |         [CC BY](https://en.wikipedia.org/wiki/CC_BY)         |      [CC BY-SA](https://en.wikipedia.org/wiki/CC_BY-SA)      |      [CC BY-NC](https://en.wikipedia.org/wiki/CC_BY-NC)      | [Copyright](https://en.wikipedia.org/wiki/Copyright), no public license |                        unpublished                         |

一般开源许可证中会说明以下权限、使用条件和责任限制：

- 商业使用(Commercial use)：该软件及其衍生产品可用于商业目的。
- 分发(Distribution)：该软件可以被分发。
- 修改(Modification)：该软件可能会被修改。
- 专利使用(Patent use)：该许可证提供了明确的专利权授予。/该许可明确声明它不授予贡献者专利的任何权利。
- 私人使用(Private use)：该软件可以私下使用和修改。
- 开源(Disclose source)：分发软件时必须开源。
- 许可及版权声明(License and copyright notice)：该软件必须随附许可证和版权声明的副本。
- 分布式网络使用(Network use is distribution)：通过网络与软件进行交互的用户被授予接收源代码副本的权利。
- 相同许可证(Same license)：分发软件时，必须以相同的许可证发布修改。在某些情况下，可以使用类似或相关的许可证
- 状态变更(State changes)：对代码所做的更改必须记录。
- 责任限制(Liability)：该许可包括责任限制。
- 商标使用(Trademark use)：该许可证明确声明它不授予商标权，即使没有此类声明的许可证可能不授予任何隐含的商标权。
- 保证(Warranty)：许可证明确声明不提供任何保证。

## Popular License

### Apache License 2.0

商业软件常用, 主要条件是要求保留原始版权和许可声明。同时向贡献者明确授予专利权。使用者可以自由修改，进行商业使用，大型项目可以不同的条款分发，没有开源要求，修改源代码需要记录变更。

### BSD 3-Clause "New" or "Revised" license

允许商业发布和销售。使用者可以自由的使用，修改源代码，也可以将修改后的代码作为开源或者专有软件再发布。主要条件是要求尊重代码作者的著作权，即包含原始版权和免责声明（二进制形式分发必须分发文档中包含版权申明及免责声明），且未经事先特别书面许可，不可以用开源代码的“作者/机构的名字”或“原来产品的名字”做市场推广。

### BSD 2-Clause "Simplified" or "FreeBSD" license

比3-Clause少一个条目，去掉了“不可以用开源代码的“作者/机构的名字”或“原来产品的名字”做市场推广。”.

### GNU General Public License

GPL不允许修改后和衍生的代码做为闭源的商业软件发布和销售。所谓的传染性协议。

### GNU Library or "Lesser" General Public License (LGPL)

允许商业软件代码动态link到LGPL类库。注意：不可以静态链接，否则你的代码也必须用LGPL协议开源。（即：商业软件可以动态使用，但不能修改）

### Mozilla Public License 2.0

修改的版本需要保持原始版权申明。编译版本需和可获得MPL协议下的源码。修改源代码需要记录变更。

### Common Development and Distribution License

商业软件可用，也可以修改。可以自行发布许可，允许公共版权使用，提供专利保护，无专利费

### Eclipse Public License version 2.0

商业软件可用，也可以修改，无需开源。不过将本程序包含在商业产品中的贡献者需要承担因代码而产生的侵权责任，及对所有其他贡献者的相关损失

---

## issues



## reference

- [Choose an open source license | Choose a License](https://choosealicense.com/)

## appendix

- 

---