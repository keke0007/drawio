# 第一节：FastAPI入门

讲师：肖斌

## 一、FastAPI框架介绍

FastAPI是一个现代、快速的Python Web框架，用于构建API。它基于Python 3.6+的类型提示特性，使得代码更加简洁且易于绶护。

**关键特性:**

**快速**：可与 NodeJS 和 Go 并肩的极高性能（归功于 Starlette 和 Pydantic）。最快的 Python web 框架之一。

**高效编码**：提高功能开发速度约 200％ 至 300％。

**更少 bug**：减少约 40％ 的人为（开发者）导致错误。
**智能**：极佳的编辑器支持。处处皆可自动补全，减少调试时间。
**简单**：设计的易于使用和学习，阅读文档的时间更短。
**简短**：使代码重复最小化。通过不同的参数声明实现丰富功能。bug 更少。
**健壮**：生产可用级别的代码。还有自动生成的交互式文档。
**标准化**：基于（并完全兼容）API 的相关开放标准：OpenAPI (以前被称为 Swagger) 和 JSON Schema。

![image.png](https://fynotefile.oss-cn-zhangjiakou.aliyuncs.com/fynote/fyfile/482/1697187079057/e54fb9a58b5546b38199a3b655824be7.png)

FastAPI 站在巨人的肩膀上：

* Starlette 用于构建 Web 部件：Starlette 是一个轻量级的 [ASGI](https://asgi.readthedocs.io/en/latest/) 框架和工具包，特别适合用来构建高性能的 asyncio 服务.
* Pydantic 用于数据的操作：python 中用于数据接口定义检查与设置管理的库。

### 什么是ASGI服务（WSGI）

![image.png](https://fynotefile.oss-cn-zhangjiakou.aliyuncs.com/fynote/fyfile/482/1697187079057/21a04142c8044e49a580a15c24245a0d.png)

![image.png](https://fynotefile.oss-cn-zhangjiakou.aliyuncs.com/fynote/fyfile/482/1697187079057/f0ae4017a0fe4420b11443d5a9773a35.png)

WSGI，（WEB SERVER GATEWAY INTERFACE），Web服务器网关接口，是一种Web服务器网关接口，它是一个Web服务器（如Nginx，uWSGI等服务器）与Web应用（如Flask框架写的程序）通信的一种规范。**当前运行在WSGI协议之上的Web框架有Bottle，Flask，Django**。

ASGI：**异步网关协议接口（** *Asynchronous Server Gateway Interface* ） ，一个介于网络协议服务和Python应用之间的标准接口，能够处理多种通用的协议类型，包括HTTP，HTTP2和WebSocket。

**WSGI和ASGI的区别**

WSGI是基于HTTP协议模式的，不支持WebSocket，而ASGI的诞生则是为了解决Python常用的WSGI不支持当前Web开发中的一些新的协议标准。同时，ASGI对于WSGI原有的模式的支持和WebSocket的扩展，即ASGI是WSGI的扩展。

![image.png](https://fynotefile.oss-cn-zhangjiakou.aliyuncs.com/fynote/fyfile/482/1697187079057/cfac5a58e220413e85c9d87071ed10e0.png)


### 1、补充Web开发

Web应用开发主要是建立在B/S架构模式下，衍生出来的一系列web应用程序，即主要是基于浏览器的应用程序开发，这也是web应用程序开发的基础，比如淘宝、京东、当当网等。Web开发在近年来，随着本身技术的突破以及移动设备的普及，基于web领域的开发，也出现了明确的岗位职责分工，一个web互联网产品中，基本上会分为web UI设计、Web前端开发以及web后端开发。

#### 1）Web前端开发

Web前端开发用到的编程语言主要有javascript，以及伴随有标记性文本语言html和样式渲染方式CSS。以及近年来衍生出来的一批优秀web前端框架，使web前端在应用构建方面的效率得到显著提升。另外nodeJs的出现，越来越多的web前端开发人员开始走入服务端编程领域，甚至在一些项目中扮演着web全栈开发的角色。

![image.png](https://fynotefile.oss-cn-zhangjiakou.aliyuncs.com/fynote/fyfile/482/1697187079057/f7713abf82cb435d84a790458680c93b.png)

#### 2）Web后端开发

Web后端开发，主要用到的语言有python、php、java等，当然随着nodeJs的兴起，也成为近年来服务端开发的另一种选择。

Web应用程序开发是基于浏览器的，浏览器本身已经解决了多平台性兼容的问题，所以web开发一般是无需考虑跨平台所面临的兼容性问题。但是，web开发领域需要解决的有另一类问题，那便是多端兼容以及融合的问题，虽然web是基于浏览器的，没有跨平台的问题，但多端的快速发展，是web开发领域的新问题，即PC端、移动端以及当下比较火热的各类小程序端。

![image.png](https://fynotefile.oss-cn-zhangjiakou.aliyuncs.com/fynote/fyfile/482/1697187079057/fe63eb7cbcfa4f7cbf9c1a35813bd8cc.png)

## 二、FastAPI安装

### 1、安装Python虚拟环境

**为什么要使用虚拟环境**

1. 项目部署时，直接导出项目对应的环境中的库就可以了；
2. 同时开发多个项目，各自项目使用的python版本不同，譬如一个是python2，另一个是python3，那么需要来回的切换python版本；
3. 当你同时开发多个项目时，特别是多个项目使用同一个库，譬如：django，但是各自项目使用的django的版本不一致时，那么你在开发这些项目时，需要来回的卸载和安装不同的版本，因为同一个python环境中，同名的库只能有一个版本。

虚拟环境的安装步骤

1. 安装好python环境
2. 安装虚拟环境库，在cmd中输入：

   ```
   pip install virtualenv 
   ```
3. 创建虚拟环境，在cmd中切换到需要创建虚拟环境的目录下，执行：

   ```
   virtualenv env_name 
   ```
4. 激活虚拟环境，在cmd中进入到 第三步创建的 env_name/Scripts 目录下，执行：

   ```
   activate
   ```

   执行成功后，在cmd中，当前输入行前面会有 (env_name) 的前缀

   在当前状态下，使用 pip 就是在虚拟环境中安装第三方库了
5. 退出虚拟环境，cmd中输入：

   ```
   deactivate
   ```

### 2、安装FastAPI

需要安装所有的可选依赖及对应功能，包括了 `uvicorn`，你可以将其用作运行代码的服务器。

pip install fastapi[all] -i https://pypi.douban.com/simple/

你也可以分开来安装。

假如你想将应用程序部署到生产环境，你可能要执行以下操作：

```
pip install fastapi
```

并且安装 `uvicorn`来作为服务器：

```
pip install "uvicorn[standard]"
```

## 三、第一个FastAPI案例

创建一个main.py

```python
from fastapi import FastAPI

app = FastAPI()


@app.get("/")
def root():
    return 'hello'


@app.get("/hello/{name}")
def say_hello(name: str):
    return {"message": f"Hello {name}"}

# 可以不要
if __name__ == "__main__":
    import uvicorn
    uvicorn.run(app, host="127.0.0.1", port=8000)
```

![image.png](https://fynotefile.oss-cn-zhangjiakou.aliyuncs.com/fynote/fyfile/482/1697187079057/a3d6e53beefe4f69a637202ee9ae8125.png)

### 1、访问接口和文档

![image.png](https://fynotefile.oss-cn-zhangjiakou.aliyuncs.com/fynote/fyfile/482/1697187079057/8720d384bfe844b6aeb09ba4bc043948.png)

![image.png](https://fynotefile.oss-cn-zhangjiakou.aliyuncs.com/fynote/fyfile/482/1697187079057/80a1beb648134d61b85d5c4923f2de5a.png)

### 2、接口文档半天打不开的解决

这个接口文档调用了一个 js脚本，这个脚本是部署在国外的，总之 就是因为这个原因导致我们没法访问了，由此我们需要把这个脚本从网上下载下来，放到本地，把此处调用国外的脚本变成调用我们自己本地的，即可。

![image.png](https://fynotefile.oss-cn-zhangjiakou.aliyuncs.com/fynote/fyfile/482/1697187079057/f5c6665a9dcf4ab7b09e363dda1dd1a0.png)

**1）下载这些国外服务器的资源到一个static目录中**

![image.png](https://fynotefile.oss-cn-zhangjiakou.aliyuncs.com/fynote/fyfile/482/1697187079057/bb68b27fdac24ad48bb95147c9feec07.png)

**2）复制到项目中**

![image.png](https://fynotefile.oss-cn-zhangjiakou.aliyuncs.com/fynote/fyfile/482/1697187079057/06bb2986b6d3418ab0c646263f484c01.png)

**3）修改fastapi的源代码**

![image.png](https://fynotefile.oss-cn-zhangjiakou.aliyuncs.com/fynote/fyfile/482/1697187079057/ac3dfdd4509a402d8877a3be306bac29.png)

**4)在app中注册static目录**

![image.png](https://fynotefile.oss-cn-zhangjiakou.aliyuncs.com/fynote/fyfile/482/1697187079057/2b9109a059704b5abb3a22d2fb079630.png)

**5）打开接口文档**

![image.png](https://fynotefile.oss-cn-zhangjiakou.aliyuncs.com/fynote/fyfile/482/1697187079057/24f030ad118940e787beb9abbba37f90.png)

### 3、总结

#### 1）uvicorn命令说明：

`uvicorn main:app` 命令含义如下:

* `main`：`main.py` 文件（一个 Python「模块」）。
* `app`：在 `main.py` 文件中通过 `app = FastAPI()` 创建的对象。
* `--reload`：让服务器在更新代码后重新启动。仅在开发时使用该选项。

#### 2）app变量

这里的变量 `app` 会是 `FastAPI` 类的一个实例 变量名字。这个实例将是创建你所有 API 的主要交互对象。

也可以修改app变量名字：

```
from fastapi import FastAPI

my_awesome_api = FastAPI()


@my_awesome_api.get("/")
def root():
    return {"message": "Hello World"}

```

![image.png](https://fynotefile.oss-cn-zhangjiakou.aliyuncs.com/fynote/fyfile/482/1697187079057/dc0bf628be474cc7a43c74316a7843f7.png)

#### 3）请求路径(路由)

请求路径指的是 URL 中从第一个 `/` 起的后半部分。所以，在一个这样的 URL 中：

```
http://example.com/items/foo
http://127.0.0.1/items/foo
```

那么，真正的请求路径（路由）是：`/items/foo`

在FastAPI中，路由的配置是通过: 装饰器 完成的。

#### 4）请求方法（操作）

在 HTTP 协议中，你可以使用以下的其中一种（或多种）请求方法 与每个路径进行通信。

通常使用：

* `POST`：创建数据。
* `GET`：读取数据。
* `PUT`：更新数据。
* `DELETE`：删除数据。
* PATCH: 修改单一数据

配置请求方法：

* `@app.post()`
* `@app.put()`
* `@app.delete()`
* @app.get()

以及更少见的：

* `@app.options()`
* `@app.head()`
* `@app.patch()`
* `@app.trace()`

#### 5)路由+请求方法 +函数的映射

* **路径** ：是 `/`
* **操作** ：是 `get`
* **函数** ：是位于「装饰器」下方的函数（位于 `@app.get("/")` 下方）

```
@app.get("/")
def root():
    return {"message": "Hello World"}
```

#### 6）返回响应

```
from fastapi import FastAPI

app = FastAPI()


@app.get("/")
def root():
    return {"message": "Hello World"}  # 返回响应

```

你还可以返回一个 `dict`、`list`，像 `str`、`int` 一样的单个值，等等。你还可以返回 Pydantic 模型。还有许多其他将会自动转换为 JSON 的对象和模型（包括 ORM 对象等）。

## 四、路由参数

你可以使用与 Python 格式化字符串相同的语法来声明路径"参数"或"变量"：

```
@app.get("/items/{item_id}")
def read_item(item_id):
    return {"item_id": item_id}
```

路径参数 `item_id` 的值将作为参数 `item_id` 传递给你的函数。所以，如果你运行示例并访问 [http://127.0.0.1:8000/items/foo](http://127.0.0.1:8000/items/foo)，将会看到如下响应：

```
{"item_id":"foo"}
```

### 1、定义参数的类型

你可以使用标准的 Python 类型标注为函数中的路径参数声明类型。

```
@app.get("/items/{item_id}")
def read_item(item_id: int):
    return {"item_id": item_id}
```

在这个例子中，`item_id` 被声明为 `int` 类型。**FastAPI** 通过上面的类型声明提供了对请求的自动"解析"。

同时还提供**数据校验**功能：

如果你通过浏览器访问 [http://127.0.0.1:8000/items/foo](http://127.0.0.1:8000/items/foo)，你会看到一个清晰可读的 HTTP 错误：

```
{
    "detail": [
        {
            "loc": [
                "path",
                "item_id"
            ],
            "msg": "value is not a valid integer",
            "type": "type_error.integer"
        }
    ]
}

```

因为路径参数 `item_id` 传入的值为 `"foo"`，它不是一个 `int`。

你可以使用同样的类型声明来声明 `str`、`float`、`bool` 以及许多其他的复合数据类型。

### 2、路由匹配的顺序

由于路由匹配*操作*是按顺序依次运行的，你需要确保路径 `/users/me` 声明在路径 `/users/{user_id}`之前：

```
@app.get("/users/me")
def read_user_me():
    return {"user_id": "the current user"}


@app.get("/users/{user_id}")
def read_user(user_id: str):
    return {"user_id": user_id}
```

否则，`/users/{user_id}` 的路径还将与 `/users/me` 相匹配，"认为"自己正在接收一个值为 `"me"` 的 `user_id` 参数。

### 3、预设值参数

如果你有一个接收路径参数的路径操作，但你希望预先设定可能的有效参数值，则可以使用标准的 Python `Enum` 类型。

Python中的枚举数据类型：是指列出有穷集合中的所有元素，即一一列举的意思。在Python中，枚举可以视为是一种数据类型，当一个变量的取值只有几种有限的情况时，我们可以将其声明为枚举类型。例如表示周几的这一变量weekday，只有七种可能的取值，我们就可以将其声明为枚举类型。

```
from enum import Enum
class Weekday(Enum):
    monday = 1
    tuesday = 2
    wednesday = 3
    thirsday = 4
    friday = 5
    saturday = 6
    sunday = 7
 
print(Weekday.wednesday)         # Weekday.wednesday  
print(type(Weekday.wednesday))   # <enum 'Weekday'>
print(Weekday.wednesday.name)    # wednesday
print(Weekday.wednesday.value)   # 3
```

```
from enum import Enum

from fastapi import FastAPI


class ModelName(str, Enum):
    alexnet = "alexnet"
    resnet = "resnet"
    lenet = "lenet"


app = FastAPI()


@app.get("/models/{model_name}")
async def get_model(model_name: ModelName):
    if model_name is ModelName.alexnet:
        return {"model_name": model_name, "message": "Deep Learning FTW!"}

    if model_name.value == "lenet":
        return {"model_name": model_name, "message": "LeCNN all the images"}

    return {"model_name": model_name, "message": "Have some residuals"}

```
