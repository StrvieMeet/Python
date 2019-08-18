### 一. **sys模块**

sys模块是与python解释器交互的一个接口，这个模块功能不是很多，练习一遍就行。

```python
sys.argv           命令行参数List，第一个元素是程序本身路径
sys.exit(n)        退出程序，正常退出时exit(0),错误退出sys.exit(1)
sys.version        获取Python解释程序的版本信息
sys.path           返回模块的搜索路径，初始化时使用PYTHONPATH环境变量的值  ***
sys.platform       返回操作系统平台名称
```

