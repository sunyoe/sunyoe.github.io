---
title: 【数据】bookmarks的转化
date: 2020-08-08 09:41:13
categories:
	- Data Analysis
tags:
	- json
	- Pandas
	- index
---

假期归来，想把家里电脑上的收藏夹搬到笔记本电脑上，其实更好的方法是建一个账户同步，但是本着能麻烦绝不方便的原则，试一下另一种方法。

## 思路

- 浏览器导出收藏夹文件：Bookmarks，该文件格式为json
- 从Bookmarks中找出网站名和网址，整理成一个表格文件
- 对这个表格做一下词云分析，看一下究竟假期看了些什么，详见[【数据】词云分析初探](https://sunyoe.github.io/2020/08/02/数据-词云分析初探/)

<!--more-->

## 动手

### 整理Bookmarks

其实已经非常简单了，把握核心就是把 json 看成一个“大”字典。

看一下原始内容：

```json
{
   "checksum": "ca7fb0d38496e7252c02db776f1f634a",
   "roots": {
      "bookmark_bar": {
         "children": [ {
            "date_added": "13199114198683132",
            "guid": "e62ed638-a320-45be-9456-eb3490b55096",
            "id": "5",
            "meta_info": {
               "last_visited_desktop": "13206982609994441"
            },
            "name": "百度",
            "type": "url",
            "url": "https://www.baidu.com/"
         }, {
            "children": [ {
               "children": [ {
                  "date_added": "13228716556104664",
                  "guid": "2011bd3e-d3df-42ae-b9b8-27420da1deb5",
                  "id": "62",
                  "name": "常见问题解答-Covid-19，问题和解答",
                  "type": "url",
                  "url": "http://www.salute.gov.it/portale/nuovocoronavirus/dettaglioFaqNuovoCoronavirus.jsp?lingua=italiano&id=228#11"
               }],
               "date_added": "13231592490508714",
               "date_modified": "13231599756212563",
               "guid": "5d867818-cf41-45a2-ad6f-25600f590419",
               "id": "105",
               "name": "EBD项目",
               "type": "folder"
            }...
 }
```

整体还是非常有规律的，对应到浏览器中就是大的收藏夹套内部分类收藏夹，收藏夹内还有文件夹，从type可以看到哪些是“url”。

> 这里突然觉得自己的方法可能有点笨，用type的方法去分类也是一个不错的选择

我的方法自然是使用字典+列表的索引来实现了，心得就是：

- 字典直接根据 “key” 键的名字来索引：

  ```python
  bm_url_data['children']
  ```

- 列表则可以根据数字来索引：

  ```python
  bm_url_data[i]['children']
  ```

这种方法确实是非常的原始，因为Bookmarks中字典列表循环嵌套，要不断来回索引，除了部分文件夹中可以简单用循环来节省一下工作量，其他方面根本就没有省多少事。

### 转为excel

上面最终取出的url和网址分别组成列表，这样就得到了两个列表。

将两个列表进行关联，并转入excel文件，用pandas实在是再好不过了！

```python
import pandas as pd
from pandas.core.frame import DataFrame

# 先根据两个列表，生成bookmarks的字典
bookmarks_dict = {'网站名': bm_name,
                  'url' : bm_url}

# 从bookmarks 生成 DataFrame数据
bookmarks_fram = DataFrame(bookmarks_dict)
print(bookmarks_fram)

# 最后直接将DataFrame数据转化到excel中
bookmarks_fram.to_excel('./bookmarks.xlsx')
```

得益于完善的封装，以及DataFrame这一数据格式的独特优势，使用这种转换方法绝对是非常简洁。

此外，还找到xlwt等python包，也可以实现数据写入excel，但是相对就要麻烦很多了。