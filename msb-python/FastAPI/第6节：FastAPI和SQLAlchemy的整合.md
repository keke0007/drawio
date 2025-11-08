# 第六节：FastAPI和SQLALchemy的整合

讲师：肖斌

# 一、ORM的查询操作

```
   	# 查找某个模型对应的那个表中所有的数据：
       all_person = session.query(Person).all()
       # 使用filter_by来做条件查询
       all_person = session.query(Person).filter_by(name='momo1').all()
       # 使用filter来做条件查询
       all_person = session.query(Person).filter(Person.name=='momo1').all()
       # 使用get方法查找数据，get方法是根据id来查找的，只会返回一条数据或者None
       person = session.query(Person).get(primary_key)
       # 使用first方法获取结果集中的第一条数据
       person = session.query(Person).first()


# 新版本
select_stmt = select(Employee).where(Employee.id > 1, Employee.sal == None)
result = session.execute(select_stmt)
for row in result.scalars():  # result里面是一行一行的数据
     print(row)
     print(row.dept.name)
result.first()

```

## 1、filter的过滤

过滤是数据提取的一个很重要的功能，以下对一些常用的过滤条件进行解释，并且这些过滤条件都是只能通过filter方法实现的：

1. **equals ： ==**

```
   news= session.query(News).filter(News.title == "title1").first()
   result = session.execute(select(Employee).where(Employee.id > 1).order_by(Employee.id))
```

2. **not equals  :  !=**

```
    query(User).filter(User.name != 'ed')
    result = session.execute(select(Employee).where(Employee.id != 1))
```

3. **like  & ilike   [不区分大小写]：**

```
    query(User).filter(User.name.like('%ed%'))
    result = session.execute(select(Employee).where(Employee.name.like('%四%')))
  
```

4. **in：**

```
   query(User).filter(User.name.in_(['ed','wendy','jack']))
   result = session.execute(select(Employee).where(Employee.name.in_(['李四', '王五'])))
```

5. **not in：**

```
     query(User).filter(~User.name.in_(['ed','wendy','jack']))
     result = session.execute(select(Employee).where(Employee.name.notin_(['李四', '王五'])))
```

6. **is null：**

```
    query(User).filter(User.name==None)
      # 或者是
   
    query(User).filter(User.name.is_(None))
    # 新版本
    result = session.execute(select(Employee).filter(Employee.dept_id.is_(None)))
```

7. **is not null:**

```
    query(User).filter(User.name != None)
     # 或者是
    
    query(User).filter(User.name.isnot(None))

    result = session.execute(select(Employee).filter(Employee.dept_id.isnot(None)))
```

8. **and：**

```Python
   query(User).filter(and_(User.name=='ed',User.fullname=='Ed Jones'))
    # 或者是传递多个参数
   
   query(User).filter(User.name=='ed',User.fullname=='Ed Jones')
   
   # 或者是通过多次filter操作
   
   query(User).filter(User.name=='ed').filter(User.fullname=='Ed Jones')

   result = session.execute(select(Employee).where(Employee.id > 2).where(Employee.dept_id.isnot(None)))
```

9. **or：**

```
   query(User).filter(or_(User.name=='ed',User.name=='wendy'))
   
   result = session.execute(select(Employee).where(or_(Employee.id > 2, Employee.dept_id.isnot(None))))
```

## 2、聚合函数

* **func.count：统计行的数量。**
* **func.avg：求平均值。**
* **func.max：求最大值。**
* **func.min：求最小值。**
* **func.sum：求和。**

```
r = session.query(func.count(News.id)).first()
print(r)

r = session.query(func.max(News.price)).first()
print(r)

r = session.query(func.min(News.price)).first()
print(r)

result = session.execute(select(func.count(Employee.id))).first()
```

## 3、分组查询

**group_by：**

根据某个字段进行分组。如想要根据年龄进行分组，来统计每个分组分别有多少人

```python
r = session.query(User.age,func.count(User.id)).group_by(User.age).all()

result = session.execute(select(Dept.name, func.count(Employee.dept_id)).join(Dept.emp_list).group_by(Dept.name))
# print(result.all())
# for item, co in result.all():
#     print(item, co)

for item in result:
      print(item.name, item.count)
```

**having：**

having是对分组查找结果作进一步过滤。如只想要看未成年人的人数，

那么可以首先对年龄进行分组统计人数，然后再对分组进行having过滤。

```
r = session.query(User.age,func.count(User.id)).group_by(User.age).having(User.age < 18).all()
```

# 二、整合

通过注入，把session对象注入到视图函数中去

```
@app.get("/test", response_class=HTMLResponse)
def test(request: Request, name: Union[str, None], session: Session = Depends(get_session)):
    all_list = session.query(Employee).all()
    return templates.TemplateResponse("result.html", {"request": request, 'emp_list': all_list})
```

```
def get_session():
    session = Session(bind=engine)
    try:
        yield session
    finally:
        session.close()
```
