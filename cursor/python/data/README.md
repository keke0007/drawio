# Python 练习数据文件说明

本目录包含所有Python练习所需的数据文件。

## 📁 数据文件列表

### 基础数据

- **basic_data.txt** - 基础数据类型练习数据（姓名、年龄、身高等）
- **text_data.txt** - 文本处理练习数据（英文段落）
- **numbers.txt** - 数字列表，用于数组和列表操作

### CSV数据文件

- **scores.csv** - 学生成绩数据（姓名、成绩）
- **students.csv** - 学生信息数据（学号、姓名、年龄、成绩）
- **student_grades.csv** - 学生课程成绩数据（学号、姓名、课程、学分、成绩）
- **employees.csv** - 员工信息数据（ID、姓名、部门、工资）
- **products.csv** - 产品信息数据（产品ID、名称、价格、库存）
- **books.csv** - 图书信息数据（ISBN、标题、作者、价格、库存）
- **orders.csv** - 订单数据（订单ID、客户ID、产品ID、数量、日期）
- **customers.csv** - 客户信息数据（客户ID、姓名、城市、电话）
- **sales.csv** - 销售数据（订单ID、日期、产品、客户、数量、单价、总价）
- **sales_data.csv** - 销售数据分析数据（日期、产品、数量、单价、销售额）
- **time_series.csv** - 时间序列数据（日期、销售额）
- **dirty_data.csv** - 脏数据（包含缺失值、重复数据，用于数据清洗练习）
- **coordinates.csv** - 坐标点数据（x、y坐标）

### JSON数据文件

- **products.json** - 产品JSON数据（包含产品列表的JSON格式）

### 日志文件

- **access.log** - Web访问日志（日期、时间、IP、方法、路径、状态码）

### 集合数据

- **set1.txt** - 集合1数据（奇数）
- **set2.txt** - 集合2数据（偶数）

## 📊 数据文件用途

### 基础语法练习
- `basic_data.txt` - 变量和数据类型
- `scores.csv` - 条件语句
- `numbers.txt` - 循环语句
- `text_data.txt` - 字符串处理
- `students.csv` - 列表和字典操作

### 函数和模块练习
- `numbers.txt` - lambda函数
- `students.csv` - 数据处理函数

### 面向对象编程练习
- `students.csv` - 学生类
- `books.csv` - 图书管理系统
- `student_grades.csv` - 成绩管理系统

### 文件操作练习
- `employees.csv` - CSV文件处理
- `products.json` - JSON文件处理
- `access.log` - 日志文件处理
- `dirty_data.csv` - 数据清洗

### 数据处理练习
- `sales_data.csv` - 数据分析
- `time_series.csv` - 时间序列分析
- `dirty_data.csv` - 数据清洗
- `orders.csv`, `customers.csv`, `products.csv` - 数据合并

## 💡 使用说明

### 读取CSV文件

```python
import csv

with open('data/scores.csv', 'r', encoding='utf-8') as f:
    reader = csv.DictReader(f)
    for row in reader:
        print(row)
```

### 读取JSON文件

```python
import json

with open('data/products.json', 'r', encoding='utf-8') as f:
    data = json.load(f)
    print(data)
```

### 读取文本文件

```python
with open('data/text_data.txt', 'r', encoding='utf-8') as f:
    content = f.read()
    print(content)
```

## ⚠️ 注意事项

1. **文件编码**：所有文件使用UTF-8编码
2. **路径问题**：运行脚本时确保在正确的目录下，或使用绝对路径
3. **数据格式**：CSV文件第一行为表头
4. **数据修改**：练习时可以修改数据，但建议保留原始数据备份

## 🔄 数据更新

如果需要添加新的数据文件：
1. 确保使用UTF-8编码
2. 遵循命名规范（小写字母、下划线）
3. 更新本README文件

## 📝 数据说明

### 数据特点

- **真实场景**：数据模拟真实业务场景
- **多样化**：包含不同类型的数据（文本、数字、日期等）
- **完整性**：部分数据文件包含缺失值，用于练习数据清洗
- **关联性**：多个文件之间存在关联关系（如订单-客户-产品）

### 数据关系

```
customers.csv (客户)
    ↓
orders.csv (订单) → products.csv (产品)
    ↓
sales.csv (销售记录)
```

---

**提示**：开始练习前，建议先查看对应的练习文档了解数据文件的用途。
