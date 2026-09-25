# 二次域壁纸

## APK 下载

从 [Releases](https://github.com/guangyunxinxikeji-boop/erciyu-wallpaper/releases) 下载最新 APK。

- 架构：arm64-v8a（64 位）
- 安装时若提示"未知来源应用"，需在系统设置中允许安装。

## 二次域壁纸站点添加指南

> 本指南自包含：添加一个新壁纸站点需要的全部说明（APP 简介、三步流程、JSON 配置结构、注意事项、验收清单、常见问题、给 AI 的提示词模板）都在这一份文档里。

### 一、这个 APP 是什么

这是一款由配置驱动的壁纸 / 图片浏览工具。

APP 本身不内置任何图片内容，显示什么、从哪里取图、能取多少张，全部由一份站点配置决定。

目前支持两种内容形态：

| 形态 | 说明 | 打开后的表现 |
|------|------|--------------|
| 单图壁纸 | 一个作品 = 一张图 | 整屏显示大图，底部出现「设为壁纸」按钮，可直接设为桌面/锁屏 |
| 多图图集 | 一个作品 = 一组图 | 打开后可连续翻看，逐张浏览，需要哪张再单独操作 |

内置站点可以随时增、删、替换。**添加新站点不需要改代码、不需要重新打包，导入一份配置即可生效。**

### 二、添加一个新站点，只需三步

```
① 复制网址  →  ② 交给 AI 生成配置  →  ③ 把配置导入 APP
```

#### 第 1 步：复制网址

在浏览器打开你想添加的壁纸站，复制下面三类网址（给得越全，生成结果越准）：

| 要复制的 | 怎么拿 | 作用 |
|----------|--------|------|
| 首页 / 列表页网址 | 打开站点首页或某个分类页 | 决定 APP 首页能刷出哪些卡片 |
| 搜索结果页网址 | 在站内随便搜一个词，复制结果页地址 | 决定全局搜索能不能用这个站 |
| 任意一个作品详情页网址 | 点进一张壁纸/一组图集，复制地址 | 决定封面、标题、作者、简介，以及它是单图还是多图 |

如果该站没有搜索功能，只给首页 + 详情页也完全可以，跳过搜索即可。

#### 第 2 步：把网址交给 AI

把你复制的网址，连同下面两样东西一起发给任意 AI（ChatGPT、DeepSeek、豆包、WorkBuddy 均可）：

- **本文「JSON 配置结构说明」一节**（AI 看得懂，你不需要看懂）；
- **文末附录的提示词模板**，把网址填进去。

AI 会返回一份 JSON 配置文本。你不需要理解里面写了什么，只要它返回的是完整的 JSON 就行。

#### 第 3 步：导入 APP

1. 把 AI 给的 JSON 保存成一个文件，文件名以 `.json` 结尾，例如 `mywallpaper.json`；
2. 把这个文件上传到能公开访问的网址上（自建服务器、对象存储、Gist raw 链接、静态托管等都可以），得到一个以 `.json` 结尾的直链；
3. 打开 APP：**个人设置 → 导入JSON → 填入这个网址 → 确定**；
4. 提示「解析成功 · 当前 N 个网站数据」即为生效，首页会自动重新加载。

### 三、配置文件里大概有什么（了解即可，不用自己写）

配置以「一个站点一个条目」的方式组织，每个条目大致包含这几块：

| 配置块 | 管什么 |
|--------|--------|
| 站点基本信息 | 站点显示名、主域名 |
| 首页 / 列表页规则 | 首页能刷出哪些卡片、封面和标题取哪里 |
| 搜索页规则 | 这个站能不能被搜到、搜索结果怎么取 |
| 详情页规则 | 封面、标题、作者、简介 |
| 图片列表规则 | 决定它是单图壁纸还是多图图集 |
| 大图读取规则 | 打开后的清晰度、占位图过滤等 |

具体怎么填由 AI 完成，你只负责给网址和导入。

### 四、JSON 配置结构说明（给 AI 的字段规范）

整份配置是一个 JSON 对象：顶层是「站点英文 key → 站点条目」。每个站点条目包含 `basicinfo`（站点基本信息）、`search`（搜索页）、`homepage`（首页/列表页）、`detailpage`（详情页，内含 `chapters` 图片列表与 `reader` 大图读取）四块。

下面是单站点的完整骨架，所有 `""` 空值字段由 AI 按目标站点的实际页面结构填写：

```json
{
  "站点英文key": {
    "basicinfo": {
      "websitename": "站点显示名",
      "websiteurl": "https://example.com/"
    },
    "search": {
      "addressurl": "https://example.com/search?q={keyword}",
      "primarynode": "",
      "childnode": "",
      "childnodeproperty": "",
      "childnodetext": "",
      "upTo": "",
      "listdatas": "",
      "data": {
        "img": {
          "primarynode": "",
          "childnode": "",
          "childnodeproperty": "",
          "childnodetext": "",
          "excludePatterns": [],
          "cleanupPatterns": []
        },
        "title": {
          "primarynode": "",
          "childnode": "",
          "childnodeproperty": "",
          "childnodetext": "",
          "excludePatterns": [],
          "cleanupPatterns": []
        },
        "switchlink": {
          "primarynode": "",
          "childnode": "",
          "childnodeproperty": "",
          "childnodetext": "",
          "excludePatterns": [],
          "cleanupPatterns": []
        }
      }
    },
    "homepage": {
      "addressurl": "https://example.com/",
      "primarynode": "",
      "childnode": "",
      "childnodeproperty": "",
      "childnodetext": "",
      "upTo": "",
      "listdatas": "",
      "data": {
        "img": {
          "primarynode": "",
          "childnode": "",
          "childnodeproperty": "",
          "childnodetext": "",
          "excludePatterns": [],
          "cleanupPatterns": []
        },
        "title": {
          "primarynode": "",
          "childnode": "",
          "childnodeproperty": "",
          "childnodetext": "",
          "excludePatterns": [],
          "cleanupPatterns": []
        },
        "switchlink": {
          "primarynode": "",
          "childnode": "",
          "childnodeproperty": "",
          "childnodetext": "",
          "excludePatterns": [],
          "cleanupPatterns": []
        }
      }
    },
    "detailpage": {
      "primarynode": "",
      "childnode": "",
      "childnodeproperty": "",
      "childnodetext": "",
      "upTo": "",
      "listdatas": "",
      "data": {
        "cover": {
          "primarynode": "",
          "childnode": "",
          "childnodeproperty": "",
          "childnodetext": "",
          "excludePatterns": [],
          "cleanupPatterns": []
        },
        "title": {
          "primarynode": "",
          "childnode": "",
          "childnodeproperty": "",
          "childnodetext": "",
          "excludePatterns": [],
          "cleanupPatterns": []
        },
        "author": {
          "primarynode": "",
          "childnode": "",
          "childnodeproperty": "",
          "childnodetext": "",
          "excludePatterns": [],
          "cleanupPatterns": []
        },
        "introduction": {
          "primarynode": "",
          "childnode": "",
          "childnodeproperty": "",
          "childnodetext": "",
          "excludePatterns": [],
          "cleanupPatterns": []
        }
      },
      "chapters": {
        "primarynode": "",
        "childnode": "",
        "childnodeproperty": "",
        "childnodetext": "",
        "upTo": "",
        "listdatas": "",
        "waitnode": "",
        "chapterOrder": 0,
        "expand_button": {
          "primarynode": "",
          "childnode": "",
          "childnodeproperty": "",
          "childnodetext": ""
        },
        "data": {
          "href": {
            "primarynode": "",
            "childnode": "",
            "childnodeproperty": "",
            "childnodetext": "",
            "excludePatterns": [],
            "cleanupPatterns": []
          },
          "title": {
            "primarynode": "",
            "childnode": "",
            "childnodeproperty": "",
            "childnodetext": "",
            "excludePatterns": [],
            "cleanupPatterns": []
          }
        }
      },
      "reader": {
        "primarynode": "",
        "childnode": "",
        "childnodeproperty": "",
        "childnodetext": "",
        "excludePatterns": [],
        "cleanupPatterns": [],
        "urlReplace": []
      }
    }
  }
}
```

各块速查：

| 块 | 作用 |
|----|------|
| `basicinfo` | 站点显示名（`websitename`）与主域名（`websiteurl`），二者是导入的最低门槛，缺一不可 |
| `search` | 搜索页地址（`addressurl` 中 `{keyword}` 为搜索词占位符）及搜索结果卡的 `img`/`title`/`switchlink`（跳转链接）提取规则 |
| `homepage` | 首页/列表页地址及卡片封面图、标题、跳转链接的提取规则 |
| `detailpage` | 详情页的封面（`cover`）、标题、作者、简介提取规则 |
| `detailpage.chapters` | 作品内图片列表（多图图集用）：`href` 为图片直链，`title` 为图片说明；单图壁纸站可留空 |
| `detailpage.reader` | 大图读取规则：图片 URL 的过滤（`excludePatterns`/`cleanupPatterns`）与替换（`urlReplace`） |
| `upTo` | 六要素之一：从匹配的子元素向上查找祖先元素，作为列表容器（在此范围内再找 `listdatas`） |
| `listdatas` | 列表项 CSS 选择器，兼作页面渲染的等待选择器 |
| `waitnode` | 异步渲染等待节点，按站点需要填写 |

### 五、导入时必须注意的 4 件事（很重要）

1. **只认网址，不认文本**
   APP 的导入框只能填 `http/https` 开头的网址，不能粘贴 JSON 内容；网址必须以 `.json` 结尾，否则会提示「格式错误」。

2. **导入是整体替换，不是追加**
   新配置会完全覆盖现有的站点列表。务必让 AI 在现有配置的基础上追加新站点，而不是只输出一个新站点，否则旧站点会全部消失。

3. **最低门槛**
   每个站点必须至少填了「站点名称」和「首页地址」，否则整份配置会被判为「JSON格式错误」，一条都不导入。

4. **收藏不会丢**
   导入只重置站点配置和浏览历史，收藏夹、卡片缓存、作品详情缓存都会保留，可以放心导入。

### 六、导入后怎么验收

按顺序自查，有问题把现象反馈给 AI 再改一版配置：

- [ ] 首页/列表页能刷出卡片，封面图正常显示
- [ ] 点进作品：单图站整屏显示且底部有「设为壁纸」；多图站能连续翻看
- [ ] 搜索页能搜出该站结果（站点本身没有搜索功能则属正常）
- [ ] 长按/菜单里的下载、设为壁纸可用

### 七、常见问题

| 现象 | 原因与处理 |
|------|------------|
| 提示「地址错误」 | 网址不是 `http/https` 开头，或缺少域名 |
| 提示「格式错误」 | 网址没有以 `.json` 结尾，改成 `.json` 结尾的直链 |
| 提示「JSON下载失败」 | 链接需要登录/跳转/防盗链，或不是直接返回原始 JSON 文本；换一个能直接返回文件内容的直链 |
| 提示「JSON格式错误」 | 某个站点缺「站点名称」或「首页地址」，或文件不是合法 JSON；把提示原样发给 AI 让它修 |
| 导入成功但首页空白 | 该站有 Cloudflare 等反爬，或列表是纯 JS 渲染；让 AI 换一种取数方式重新生成 |
| 列表有卡片但图裂 | 图片走了防盗链或懒加载；把现象告诉 AI，让它调整图片读取规则 |
| 导入后原来的站点没了 | 配置被整体覆盖；让 AI 在现有完整配置上追加新站点后重新导入 |
| 搜索没结果 | 该站搜索是标签式的，只认站内真实标签词；或该站本就没有搜索能力 |
| 预览正常但无法下载/设为壁纸 | 图片 CDN 有防盗链校验；让 AI 补充站点请求头相关配置 |

### 八、免责声明

请务必使用已获版权方授权或允许转载的数据源。使用者须对导入内容的版权合法性承担全部法律责任，本应用不承担因用户导入内容引发的任何侵权责任。

---

## 附录：给 AI 的提示词模板

复制下面这段，把网址填进去，连同本文（或其中「JSON 配置结构说明」一节）一起发给 AI：

```text
我要给一款壁纸 APP 添加一个新的图片站点，请你帮我生成它的站点配置 JSON。

【配置结构参考】
按附件文档的 JSON 结构骨架输出（一个站点条目包含 basicinfo / search / homepage / detailpage 四块，detailpage 内含 chapters 与 reader）。

【站点网址】
- 首页/列表页：<把复制的首页网址粘贴到这里>
- 搜索结果页：<把复制的搜索页网址粘贴到这里，没有就写"无">
- 作品详情页示例：<把复制的详情页网址粘贴到这里>

【这个站的内容形态】
- <单图壁纸 / 多图图集 / 两种都有，不确定就写"请你自己判断">

【输出要求】
1. 严格按上面给定的结构骨架输出，不要自创字段；
2. 在下面这份现有配置的基础上**追加**新站点，不要删掉已有站点（如果你看不到现有配置，就只输出新站点，并提醒我手动合并）；
3. 只输出 JSON 本身，不要解释、不要加 markdown 代码块以外的内容；
4. 顺便用一句话告诉我：这个站你判定为单图还是多图，以及首页/搜索/详情页分别取到了几条。
```
