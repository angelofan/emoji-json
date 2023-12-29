# emoji-json

## 介绍

此仓库有适配了 简体中文 的 emoji.json 数据源，与 unicode 联盟的数据源保持同步并版本对齐。

另外 此仓库也包含了 [emoji.json](https://github.com/amio/emoji.json) 的数据转换器，包括了所有从 源数据 转换的程序。

将 [emoji.json](https://github.com/amio/emoji.json) 的数据中增加一些字段的 i18n 属性，并在 i18n 属性中添加简体中文翻译。

### 如何与 unicode 联盟的数据源保持同步并版本对齐

正如上文所说，此仓库是基于 [emoji.json](https://github.com/amio/emoji.json) 数据进行二次转换处理的。[emoji.json](https://github.com/amio/emoji.json) 是基于 unicode 联盟发布的的数据源 emoji-test.txt 进行 json 处理的。

所以此仓库视为是间接地与 unicode 联盟的数据源进行同步，并且导出的数据与其版本号保持一致。

## 功能描述

### 抽取可供本地化的字段

首先，程序会将 `group` 、 `subgroup` 、 `name` 三个属性值提取出来，以供本地化翻译和校验。

翻译完成之后，在界面中进行设置，设置完成之后，即可进行数据导出操作。

### 转换后的数据导出

原数据为数组扁平式结构，此程序提供 2 种转换结果：

- 数组结构：扁平式 emoji 列表，与原数据结构一致，增加了 `group_i18n` 、 `subgroup_i18n` 、 `name_i18n` 三个属性，用于存储本地化语言。删除了 category 属性，以为这个属性全部是由 `group` 和 `subgroup` 拼接而成的。
- 树形结构：嵌套式 emoji 列表，基于数组结构按照 `group` 和 `subgroup` 对数据进行了树形嵌套处理。嵌套后的数据结构为三层树形结构，外面两层都是 `[name: '', name_i18n: [], list: []]` 结构， `list` 是内层数组。第一层是 主分组，第二层是 次分组，第三层是 emoji 列表。最内层的 emoji 列表删除了用于分组的 `group` 和 `subgroup` 及其 i18n 属性。

树形结构 是基于 数组结构 的运行结果进行二次处理的，所以在运行 树形结构 之前，要先运行一次 数组结构。

## 使用说明

### 使用 npm 安装

```bash
npm install @angelofana/emoji-json
```

```javascript
// 数组结构
var emoji_array = require('emoji-array.json');
console.log(emoji_array[0]);
/*
{
	"codes": "1F600",
	"char": "😀",
	"name": "grinning face",
	"group": "Smileys & Emotion",
	"subgroup": "face-smiling",
	"group_i18n": {
		"en": "Smileys & Emotion",
		"zh_CN": "表情与情感"
	},
	"subgroup_i18n": {
		"en": "face-smiling",
		"zh_CN": "脸-微笑"
	},
	"name_i18n": {
		"en": "grinning face",
		"zh_CN": "笑脸"
	}
}
*/

// 树形结构
var emoji_tree = require('emoji-tree.json');
console.log(emoji_tree[0][0]);
/*
{
	"codes": "1F600",
	"char": "😀",
	"name": "grinning face",
	"name_i18n": {
		"en": "grinning face",
		"zh_CN": "笑脸"
	}
}
*/
```

### 使用 CDN 版本

- [https://unpkg.com/@angelofana/emoji-json/emoji-array.json](https://unpkg.com/@angelofana/emoji-json/emoji-array.json)
- [https://unpkg.com/@angelofana/emoji-json/emoji-tree.json](https://unpkg.com/@angelofana/emoji-json/emoji-tree.json)

### 直接使用转换好的文件

直接使用 `dist` 目录中的文件

### 自己动手进行转换

#### 下载到本地进行转换

1.  克隆仓库
2.  直接运行 index.html 文件
3.  按 F12 打开控制台，查看提示并完成转换

## 开源许可证

MIT