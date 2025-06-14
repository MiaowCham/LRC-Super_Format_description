# LRC - Super 格式规范 v2.0

> [!note]
> 本文档现已迁移至 [Repository_for_MiaowCham](https://github.com/MiaowCham/Repository_for_MiaowCham/blob/main/docs/LRC%20-%20Super%20%E6%A0%BC%E5%BC%8F%E8%A7%84%E8%8C%83v2.md)<br>
> 该仓库将被归档

一种基于 LRC 的逐字歌词格式，参考了 TTML 和 Lyricify Syllable<br>
LRC - Super 为了解决普通歌词可读性差、兼容性低的问题而诞生<br>
LRC - Super 歌词文件可以直接使用 `lrc` 后缀，也可以使用单独的 `lrcs`

## 头部信息
歌词头部可选的包含歌词相关信息，如：  
```
[ti:Counting Stars]
[ar:OneRepublic]
[al:Native]
[by:Wang]
[offset:0]
[v1:singer1]
[v2:singer2]
[QQMusic:12345]
```
- `[ti:Counting Stars]` 为歌曲标题
- `[ar:OneRepublic]` 为歌手名
- `[by:Wang]` 为歌词制作者
- `[offset:0]` 为歌曲偏移量（单位毫秒）
- `[v1:singer1]` 用于表明演唱者 ID 所代表的歌手
- `[QQMusic:12345]` 歌曲对应平台及平台 ID

*注意：该内容可以不填，不要包含重复标签。*  

## 歌词
LRC - Super 歌词的标准格式为：
```
[start]agent:<start,duration>Word <start,duration>word
```
- `agent` 用于标记该句演唱歌词的艺人的 ID，以 `v1` `v2`等来标记。<br>
一般 `v1` 对应左视图，`v2` 对应右视图，`bg` 对应背景人声。<br>
若只有一个演唱者可以不填。
- `start` 为起始时间,其时间戳使用同步多媒体集成语言（SMIL）表示。<br>
    一般情况下使用SMIL表示，可提供完整或部分时钟值：<br>
    `hh:mm:ss.sss（时:分:秒.毫秒）` <br>
    其中，小时和毫秒可不提供。<br>
- `duration` 为时长。使用毫秒计数。如果未填写 `duration` 则将后一个单词的开始时间作为结束时间，并需要在句子结尾增加 `<end>` 标记句子的结束。（兼容增强型lrc）<br>
- `<>` 中的时间戳为后方单词的起始时间和时长。

### **注意和示例：**<br>
即使背景人声的起始时间在主句前面，背景人声也得在主句的后一行，如：
```
[00:10.856]v1:<10.856,342>Had <11.198,160>to <11.358,363>have
[00:08.705]bg:<8.705,456>High, <9.161,782>high <9.943,709>hopes
```

### **逐句歌词：**<br>
如果想使用 LRC - Super 记录逐句而非逐字歌词，直接让时间轴直接标记整句即可：
```
[start]agent:<start,duration>Lyrics
```
或者：
```
[start]agent:Lyrics[end]
```

## 翻译
LRC - Super 会将 **拥有相同开始时间** 且 **更靠后的非逐字歌词** 视为翻译<br>

可以在歌词行下方直接输入翻译（翻译必须和原句一一对应）
```
[00:31.379]v1:<00:31.379,275>Did <00:31.654,159>it <00:31.813,259>really <00:32.072,751>happen
那件事真的发生过吗
```
也可以在翻译行前添加时间轴，使翻译更精准
```
[00:32.823]v1:<00:32.823,329>Or <00:33.152,184>were <00:33.336,343>they <00:33.679,570>pieces <00:34.249,299>thrown <00:34.548,1663>around
[00:32.823]亦或是支离破碎的回忆
```

### 外置 lrc 翻译格式同上，但必须要有对应的时间轴

## 兼容
LRC - Super 直接兼容 `逐行lrc` 和 `增强型lrc`；兼容 `逐字lrc`，但不能与其他格式混用<br>
不兼容 `lys` `ttml`,但由于特性相似可以实现转换<br>
但只有完整且规范的 LRC - Super 才能激活其完整特性（对唱视图、背景人声等）
