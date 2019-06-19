安装:

```python
pip  install flask
```

简单启动:

 ```python
from flask import Flask
app = Falsk(__name__)
app.run()
 ```

视图和路由启动:

```python
from flask import Flask,render_template,redircet
app = Falsk(__name__)
@app.route('/') # 这是一个路由
def index():
    return '字符串内容' #  和Django中HttpResponse 一样

# 自己手动创建一个templates文件夹然后右键这个文件夹,找打(Mark Directory as)倒数第四个 

@app.route('/login')
def login():
    return render_template('login.html')  #返回一个前端模板中的内容

@app.route('ret')
def ret():
    return redirect('/login')  # 放的是一个路由 返回的是一个url 浏览器进行重定向
```

返回文件流

```python
from flask import Flask,send_file
app = Flask(__name__)
@app.route('/get_file')
def get_file():
    return send_file('文件名') #存放的是一个图片的话就会将文件中的流发送给浏览器,浏览器能够检测到是什么类型,然后觉得是否进行渲染 
```

返回json数据

```python
from flask import Flask,jsonify
app = Flask(__name__)
@app.route('/get_json')
def get_json():
    return jsonify
```

接受form表单参数:

```python
from flask import Flask,request
app = Flask(__name__)
@app.route('/reg,methods=['POST','GET'])  methods是接受发送的请求方式,可以是多个存放在列表当中
def func():
	print(request.form)  # 接受前端发送的post请求和数据  获取的是一个字典
    print(request.args)  # 接受前端发送的get请求和url携带的数据  获取的是一个字典
    print(request.cookies) # 接受浏览器的cookie
    print(request.url)     # 获取前端页面的url
    print(reqeust.path)    # 获取url的路由地址
    print(request.to_dict()) # 获取整个字典
    print(request.files)    # 这样获取到的是一个字典
    file = request.files.get('my_file') # my_file是前端发送请求是咱们定义的名字
    file.save(file.filename) #这样是将前端页面的内容保存,file.filename是将文件的名字获取到当做要保存的文	  件名字
    print(request.data)    # 接受请求里的内容,如果不是可以识别的类型就存放在data中,data中的获取的可以序		列化
    print(request.json)    #接受请求体里的内容,请求头明确的表明是application/json时,数据从json中获取
app.run(debug = True)      # 这样是检测到代码的变化后就重新启动服务
```

jinjia2语法:

```python
在前端模板部分使用 {{name}} 接受变量
定义函数:
    {% macro 函数名() %}
    	函数内容
    {%endmacro%}
 	函数调用
    
在后端定义函数,全局使用
@app.template_global()
def func():
    函数体
func()

后端写的内容到前段能够进行渲染
导入 Markup 
然后将内容放到Markup里边
```

路由:

``` python
多个视图函数需要添加一个功能的时候我们可以使用装饰器来解决,使用装饰器会出现一个重名情况,
我们可以使用from functools import wraps 来解决这个问题
我们也可以在@app.route('/',endpoint = '新的函数名字')
endpoint 可以和 url_for配合使用,通过url_for能够获取到路由地址
redirect_to = "重定向的路由地址" #返回的是301 永久重定向 在进入视图函数之前就进行跳转了 
```

动态路由参数:

```python
@app.route('/login<int:n>') # 接受数字参数  字符串不行
@app.route('/login<string:a>') # 接受字符串和数字参数  
@pp.route('/login<a>')  #不指定类型默认就是string
@app.route('/login<file_name>')
```

Flask蓝图:

```python
蓝图就是可以将一些用户的请求分布成很独立的,然后做二级路由分布对应视图
蓝图就是一个不能启动的Flask实例
首先我们可以写一个app这样的蓝图,然后将这个蓝图和我们的Flask的进行绑定
```

特殊装饰器:

```python
@app.before_request      #在url请求之间先执行这个视图
@app.after_request       #在urk请求之后最后执行这个视图

执行顺序  before - view - after
```

