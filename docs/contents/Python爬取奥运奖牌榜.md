


@[TOC](Python爬取奥运奖牌榜)

## 场景介绍
 - 数据截止于2021-07-31
 - 使用urllib库爬取东京奥运会奖牌榜
 - 使用pandas库快速导入、处理网页中的表格
 - 查看奖牌榜网址(url='https://olympics.com/tokyo-2020/olympic-games/zh/results/all-sports/medal-standings.htm')
## 实例环境及工具
- Python开发环境、Jupyter notebook编辑器
- urllibs是python自带的库
- pandas库需要自行安装``pip install pandas``
## 爬取奖牌榜
1. 导入相关库
	```python
	import pandas as ps
	from urllib import request
	```
2. 获取网页页面
	```python
	url = 'https://olympics.com/tokyo-2020/olympic-games/zh/results/all-sports/medal-standings.htm'
	page1 = request.urlopen(url)
	```
3. 读取网页数据
	```python
	html = page1.read()
	print(html)
	```
	![在这里插入图片描述](https://img-blog.csdnimg.cn/1a4690fc58c7436b926a22c332effa68.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQzNjU2Mzk3,size_16,color_FFFFFF,t_70)

4. 读取表格数据
	```python
	df = pd.read_html(html)[0] # 转换成Pandas数据
	df1 = df[:10] # 仅查看排名前十
	df1
	```
	![在这里插入图片描述](https://img-blog.csdnimg.cn/a6aab29450a548bc8959ad68e8f07ce8.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQzNjU2Mzk3,size_16,color_FFFFFF,t_70)

5. 转换嵌套字典
	```python
	df1.T.to_dict().values()   # 转换成嵌套字典的格式
	```
	![在这里插入图片描述](https://img-blog.csdnimg.cn/9d9c1a58df7348e9b84a75e2e8ad917e.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQzNjU2Mzk3,size_16,color_FFFFFF,t_70)

## 代码汇总
```python
	import pandas as ps
	from urllib import request
	url = 'https://olympics.com/tokyo-2020/olympic-games/zh/results/all-sports/medal-standings.htm'
	page1 = request.urlopen(url).read() # 获取网页
	df = pd.read_html(html)[0] # 转换成Pandas数据
	df1 = df[:3] # 仅查看排名前三
	list(df1.T.to_dict().values())   # 转换成列表嵌套字典的格式
```
```json
[{'排名': 1,
  '国家奥委会': '中国',
  'Unnamed: 2': 22,
  'Unnamed: 3': 13,
  'Unnamed: 4': 12,
  '总分': 47,
  '按总数排名': 2,
  '国家奥委会代码': 'CHN'},
 {'排名': 2,
  '国家奥委会': '美国',
  'Unnamed: 2': 19,
  'Unnamed: 3': 20,
  'Unnamed: 4': 13,
  '总分': 52,
  '按总数排名': 1,
  '国家奥委会代码': 'USA'},
 {'排名': 3,
  '国家奥委会': '日本',
  'Unnamed: 2': 17,
  'Unnamed: 3': 5,
  'Unnamed: 4': 8,
  '总分': 30,
  '按总数排名': 5,
  '国家奥委会代码': 'JPN'}]

```