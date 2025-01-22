---
title: python---爬取练习
date: 2020-10-21 11:52:14
author: ReadPond
tags:
- python
categories:
- python
keywords:
- 爬虫
top: false
index_img: /img/202301181446480.png
banner_img: /img/202301181446480.png
---

```python
'''
# -*- coding: utf-8 -*-
# @Time    : 2022/10/29 23:49
# @Author  : ReadPond
# @Comment : 爬取豆瓣电影详情---->批量爬取
'''
import requests

head = {        # 设定一个头部信息
    "User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/107.0.0.0 Safari/537.36 Edg/107.0.1418.24"
}

url = "https://movie.douban.com/j/chart/top_list"   # 根据抓包工具分析我们爬取内容的地址


'''
通过抓包工具分析得知，当前网站的电影排行榜中的内容为动态数据，且url地址也不是正常官网地址，
电影名称和评分等信息没有存放在主体页面中，而是通过json请求到后端数据库中，所以我们这里需要根据当前网站定义
传入的数据（每个网站的数据方式不同，这里需要具体网站具体分析）
'''
param = {
    "type": "12",       # 表示电影类型，该网站电影类型数为31个
    "interval_id": "100:90",
    "action":"",
    "start":"0" ,  # 该网站 start表示从第几条数据开始
    "limit": "10"   # 读取几条数据
}
data_html = requests.get(url=url,headers=head,params=param)     # 发起请求

# 获取响应数据
html_text = data_html.json() # json()可以将获取到的json格式的字符串进行反序列化

'''
通过对数据分析，我们得到评分数据以字典的方式存储，每一个电影都以一个object存储
'''
# 将数据保存到本地
file = open(file='电影排名.txt',mode='w',encoding='utf-8')
for dic in html_text:
    types = dic['types']
    title = dic['title']
    score = dic['score']
    release_date = dic['release_date']
    file.write(f"{types}:{title}:{score}-上映时间：{release_date}"+"\n")
    print(f"{title}保存成功~~")
file.close()


```

