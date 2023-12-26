---
title: UPM 包制作与 Git url 使用
date: 2023-12-5
---



# UPM

极致简易的 UPM 管理上手教程。

## Package

### Git

在远程仓库上新建一个空项目，然后在 Unity 项目中 Asset 下 `git clone`。

### package.json

理论上仓库内只要按正确的格式编写 `package.json` 就能直接作为 UPM 包导入了，但不推荐。

```json
{
  "name": "com.enakit.utility",
  "version": "1.0.0",
  "displayName": "EnaKit.Utility",
  "description": "这是 EnaKit 开发过程中产生的一些的工具、拓展、模板类，除了 System、UnityEngine 不依赖其他项。",
  "unity": "2021.3",
  "dependencies": {
  },
  "keywords": ["enakit", "EnaKit"],
  "author": {
    "name": "Yoziya",
    "email": "yoziya@qq.com"
  },
  "license": "GPL",
  "repository": "https://github.com/yoziya/Framework",
  "documentationUrl": "https://github.com/yoziya/Framework/blob/main/README.md",
  "changelogUrl": "https://github.com/yoziya/Framework/blob/main/CHANGELOG.md"
}
```

- `name`: Package 标识符。
- `displayName`: Package 实际显示的名称。
- `dependencies`: Package 依赖的其他包和对应版本号。例如 `"com.unity.modules.xr": "1.0.0"`。
- `keywords`: UPM 中搜索该包用的关键字。
- `License`: Package 使用的许可证类型，例如 `"MIT"` 或 `"Apache-2.0"`。
- `Repository`: Package 源代码所在的仓库信息，通常是一个Git仓库的URL。
- `DocumentationURL`: 指向包文档的链接。
- `changelogUrl`: 指向包变更日志的链接。

### layout

[官方推荐的目录结构](https://docs.unity3d.com/Manual/cus-layout.html)

```
<package-root>
  ├── package.json
  ├── README.md
  ├── CHANGELOG.md
  ├── LICENSE.md
  ├── Third Party Notices.md
  ├── Editor
  │   ├── <company-name>.<package-name>.Editor.asmdef
  │   └── EditorExample.cs
  ├── Runtime
  │   ├── <company-name>.<package-name>.asmdef
  │   └── RuntimeExample.cs
  ├── Tests
  │   ├── Editor
  │   │   ├── <company-name>.<package-name>.Editor.Tests.asmdef
  │   │   └── EditorExampleTest.cs
  │   └── Runtime
  │        ├── <company-name>.<package-name>.Tests.asmdef
  │        └── RuntimeExampleTest.cs
  ├── Samples~
  │        ├── SampleFolder1
  │        ├── SampleFolder2
  │        └── ...
  └── Documentation~
       └── <package-name>.md
```

完成包的内容制作后 `git push` 推到远端即可。

## Git url import

`Window → Package Manager → "+" → Add package from git URL`

直接添加仓库地址。

---

## issues



## reference

- [Unity - Manual: Unity's Package Manager (unity3d.com)](https://docs.unity3d.com/Manual/Packages.html)

## appendix



---