# 第二节：FastAPI中请求数据

讲师：肖斌

# 一、URL请求参数

url请求参数是通过url请求地址携带的，例如，在以下 url 中：

```
http://127.0.0.1:8000/items/?skip=0&limit=10
```

这些请求参数是键值对的集合，这些键值对位于 URL 的 `？` 之后，并以 `&` 符号分隔。

请求参数为：

* `skip`：对应的值为 `0`
* `limit`：对应的值为 `10`

当你为它们声明了 Python 类型（在上面的示例中为 `int`）时，它们将转换为该类型并针对该类型进行校验。

```python
from fastapi import FastAPI

app = FastAPI()

fake_items_db = [{"item_name": "Foo"}, {"item_name": "Bar"}, {"item_name": "Baz"}]


@app.get("/items/")
def read_item(skip: int = 0, limit: int = 10):
    return fake_items_db[skip : skip + limit]

```

URL请求参数不是路径的固定部分，因此它们可以是可选的，并且可以有默认值。

在上面的示例中，它们具有 `skip=0` 和 `limit=10` 的默认值。你可以将它们的默认值设置为 `None`。

```python
from typing import Union

from fastapi import FastAPI

app = FastAPI()


@app.get("/items/{item_id}")
async def read_item(item_id: str, q: Union[str, None] = None, short: bool = False):
    item = {"item_id": item_id}
    if q:
        item.update({"q": q})
    if not short:
        item.update(
            {"description": "This is an amazing item that has a long description"}
        )
    return item

```

Union是Python 3.10版本引入的新类型,它允许创建多种类型的联合(union)。，Union类型是一种用于表示一个变量可以是多个不同类型之一的类型注解。它是typing模块中的一个类，可以与其他类型一起使用，以指定一个变量可以接受的多个类型。

**Union类型的语法为：Union[type1, type2, ...]**
这里的type1、type2等代表要包含在Union类型中的类型。

**注意：当你想让一个查询参数成为必需的，不声明任何默认值就可以：**

```python
@app.get("/items/{item_id}")
def read_user_item(
    item_id: str, needy: str, skip: int = 0, limit: Union[int, None] = None
):
    item = {"item_id": item_id, "needy": needy, "skip": skip, "limit": limit}
    return item
```

在这个例子中，有3个查询参数：

* `needy`，一个必需的 `str` 类型参数。
* `skip`，一个默认值为 `0` 的 `int` 类型参数，也是可选的。
* `limit`，一个可选的 `int` 类型参数。

# 二、请求数据校验

## 1、Query校验数据

**Query**是 **FastAPI**专门用来装饰请求参数的类，也可以提供校验。

### 1）默认值设置，和参数接口描述

```python
@app.get("/items/")
async def read_items(q: Union[str, None] = Query(default=None, description="参数q可以传入字符串或者不传")):
    query_items = {"q": q}
    return query_items
```

**description**是 **Query**中的一个字段，用来描述该参数；

### 2）字符串长度校验

```python
@app.get("/items2/")
async def read_items(q: Union[str, None] = Query(default=None, max_length=50, min_length=3)):
    query_items = {"q": q}
    return query_items
```

注意：**max_length**和 **min_length**仅可用于 str 类型的查询参数。

### 3)正则表达式校验

```python
@app.get("/items3/")
async def read_items(
    q: Union[str, None] = Query(default=None, regex="^laoxiao$")
):
    query_items = {"q": q}
    return query_items
```

也就是说该接口的查询参数只能是" **laoxiao**"，不然就会报错，当然其他的正则表达式都是可以拿来用的！

### 4）数值大小校验

如果参数的类型是int，float就可以采用Query进行大小校验，或者范围校验。

```python
@app.get("/test4/")
async def read_items4(id: int = Query(description="id参数必须是0到1000之间", gt=0, le=1000), q: str = Query(default=None)):
    results = {"item_id": id}
    if q:
        results.update({"q": q})
    return results
```

* gt：大于（greater than）
* ge：大于等于（greater than or equal）
* lt：小于（less than）
* le：小于等于（less than or equal）

# 三、url请求参数中传入一组值

我们还可以使用 Query 去接收一组值。使用 List[str] 来接收一组值，里面的str可以根据需求随意调换。

```python
@app.get("/test5/")
async def read_items(q: Union[List[str], None] = Query(default=None, description='q可以传入多个值')):
    query_items = {"q": q}
    return query_items
```

![image.png](https://fynotefile.oss-cn-zhangjiakou.aliyuncs.com/fynote/fyfile/482/1697450036090/abc4147b1f71429e85b3ff151274d161.png)

这是因为参数 **q**中以一个 **Python list** 的形式接收到查询参数 **q** 的多个值。
