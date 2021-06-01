### python操作es示例

1、首先在命令行启动elasticsearch：

```
切换至elasticsearch的安装目录的bin目录中，输入：elasticsearch，启动es
```

2、安装python需要的包：`pip install elasticsearch`

3、使用python创建索引：

```python
# -*- coding: utf-8 -*-
from elasticsearch import Elasticsearch

es = Elasticsearch()
# 创建索引
result = es.indices.create(index="news", ignore=400)
print(result)
```

4、插入数据

```python
data = {"title": "美国留给伊拉克的是烂摊子吗？", 'url': 'http://view.news.qq.com/zt2011/usa_iraq/index.htm'}
# result = es.create(index="news", doc_type="politics", id=1, body=data)  # 使用create方法插入数据
result = es.index(index="news", doc_type="politics", body=data)  # 使用index方法插入数据
```

5、更新数据

```python
data = {
    'title': '美国留给伊拉克的是个烂摊子吗',
    'url': 'http://view.news.qq.com/zt2011/usa_iraq/index.htm',
    'date': '2011-12-16'
}
result = es.update(index='news', doc_type='politics', body={"doc": data}, id=1)
print(result)
```

