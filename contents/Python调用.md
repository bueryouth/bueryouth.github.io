

@[TOC](Python调用另一个.py文件中的类和函数)

## 场景介绍
1. 在**demo01.py**中编写一个add()加法方法，在**demo02.py**中调用demo01.py的add()方法做一组加法测试。
2. 在**demo03.py**中编写一个dm03类,dm03类定义一个add()加法方法，在**demo04.py**中引入demo03.py的dm03类，创建实例调用的add()方法做一组加法测试。
2. 在目录 "E:\python" 下的**demo05.py**中编写一个dm05类,dm05类定义一个add()加法方法，在**demo06.py**中引入demo05.py的dm05类，创建实例调用的add()方法做一组加法测试。
## 实例环境及工具
- Python开发环境、Sublime_text文本编辑器
## 相同文件夹下的调用
- 文件之间调用方法
1. 编写**demo01.py**
	```python
	def add( a, b):
		# 返回a+b的和
		return a + b
	```
2. 编写**demo02.py**
	```python
	# 直接引入文件名
	import demo01
	# 输出测试用例
	print(demo01.add(12, 2))
	```
3. 运行测试**demo02.py**
	```python
	14
	[Finished in 172ms]
	```
- 文件之间调用类
1. 编写**demo03.py**
	```python
	class dm03():
		def add( a, b):
			return a + b
	```
2. 编写**demo04.py**
	```python
	# 引入demo01的dm01类
	from demo03 import dm03
	# 创建对象
	dm = dm03
	# 调用add()方法
	print(dm.add(10, 2))
	```
3. 运行测试**demo04.py**
	```python
	12
	[Finished in 187ms]
	```
## 不同文件夹下的调用
- 文件之间调用类
1. 在目录 "E:\python" 下编写**demo05.py**
	```python
	class dm05():
		def add( a, b):
			return a + b
	```
2. 编写**demo06.py**
	```python
	# 引入sys模块
	import sys
	sys.path.append(r'E:\python')
	# 引入demo01的dm01类（会在sys.path模块列表中查找）
	from demo05 import dm05
	# 创建对象
	dm = dm05
	# 调用add()方法
	print(dm.add(8, 2))
	```
3. 运行测试**demo06.py**
	```python
	10
	[Finished in 156ms]
	```