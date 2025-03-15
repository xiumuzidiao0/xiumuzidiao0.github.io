---
Markdown 语法简单直观，适用于快速写作和排版。你可以在 GitHub、Notion、Typora、Obsidian 等工具中使用它！
Markdown 是一种轻量级标记语言，常用于编写文档、笔记、博客等。它使用简单的文本格式来表示不同的排版效果，比如标题、列表、代码块等。以下是 Markdown 语法的基本用法：


---

1.标题

使用 # 号表示标题，# 的数量代表标题的层级。
```bash
# 一级标题
## 二级标题
### 三级标题
#### 四级标题
##### 五级标题
###### 六级标题
```
效果：
# 一级标题
## 二级标题
### 三级标题
#### 四级标题
##### 五级标题
###### 六级标题
---
2.字体样式
```bash

加粗：**加粗** 或 __加粗__

斜体：*斜体* 或 _斜体_

加粗+斜体：***加粗+斜体***

删除线：~~删除线~~
```

效果：

**加粗**

*斜体*

***加粗+斜体***

~~删除线~~

---

3.列表

无序列表

使用 -、* 或 + 加空格创建无序列表：
```bash
- 项目 1
- 项目 2
  - 子项目 1
  - 子项目 2
```
效果：
- 项目 1
- 项目 2
  - 子项目 1
  - 子项目 2
有序列表

使用数字加 . 创建有序列表：
```bash
1. 第一项
2. 第二项
3. 第三项
```
效果：

1. 第一项


2. 第二项


3. 第三项




---

4. 代码

行内代码

使用反引号 ` 包裹：
```bash
`print("Hello, Markdown!")`
```
效果：
`print("Hello, Markdown!")`

代码块

使用三个反引号 ``` 包裹，并可指定语言高亮（如 python、bash）：
```bash
```python
def hello():
    print("Hello, Markdown!")
`删掉``
```
效果：

```python
def hello():
    print("Hello, Markdown!")
```

---

5.引用

使用 > 表示引用，可以嵌套：
```bash
> 这是一个引用
>> 这是嵌套引用
```
效果：

> 这是一个引用
>> 这是嵌套引用






---

6.分割线

使用 ---、*** 或 ___ 创建分割线：
```bash
---
***
___
```
效果：


---


---


---


---

7.链接和图片

链接
```bash
[百度](https://www.baidu.com)
```
效果：[百度](https://www.baidu.com)

图片
```bash
![描述文本](https://avatars.githubusercontent.com/u/107784127?v=4)
```
效果：
![描述文本](https://avatars.githubusercontent.com/u/107784127?v=4)
---

8. 表格

使用 | 分隔内容，并用 - 画表头：
```bash
| 姓名  | 年龄 | 职业  |
|------|----|------|
| 张三 | 25 | 程序员 |
| 李四 | 30 | 设计师 |
```
效果：

| 姓名  | 年龄 | 职业  |
|------|----|------|
| 张三 | 25 | 程序员 |
| 李四 | 30 | 设计师 |

---
9.任务列表

使用 - [ ] 创建未完成任务，- [x] 表示已完成任务：
```bash
- [x] 已完成任务
- [ ] 未完成任务
```
效果：

- [x] 已完成任务

- [ ] 未完成任务



---

10.HTML 支持

Markdown 允许嵌入 HTML 代码，例如：
```bash
<d删iv style="color: red;">这是一段红色文本</d删iv>
```
效果：

<div style="color: red;">这是一段红色文本</div>

---

11.视频

Markdown 本身不支持直接嵌入视频，但可以通过 HTML标签或外部链接的方式插入视频。

1.使用 HTML video标签（适用于本地或在线视频）
```bash
<video src="video.mp4" controls width="600"></video>
```
这个方法适用于支持 HTML 的 Markdown 渲染器（如某些博客或网站）。

2. 插入 YouTube 视频（适用于 GitHub、博客等）
```bash
[![视频标题](https://img.youtube.com/vi/视频ID/0.jpg)](https://www.youtube.com/watch?v=视频ID)
```
替换 视频ID 为 YouTube 视频的 ID，例如 dQw4w9WgXcQ。

这个方法实际上是给视频设置一个可点击的缩略图，用户点击后跳转到 YouTube 播放。

示例：

[![点击观看视频](https://img.youtube.com/vi/dQw4w9WgXcQ/0.jpg)](https://www.youtube.com/watch?v=dQw4w9WgXcQ)

3.使用 iframe（适用于支持 HTML 的平台）
```bash
<iframe width="560" height="315" src="https://www.youtube.com/embed/视频ID" frameborder="0" allowfullscreen></iframe>
```
这个方法适用于支持 HTML 的 Markdown 渲染器，例如某些博客平台或 Notion，但 GitHub 不支持。


---

不同平台对 Markdown 的支持程度不同，如果你使用的是 GitHub、Typora 等，一般推荐使用 外部链接，如果是自己搭建的博客或 HTML 支持的 Markdown 环境，可以使用 < video> 或 < iframe> 方式。