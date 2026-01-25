# 数据文件说明

## 📁 目录结构

```
data/
├── input/          # 输入数据文件
├── output/         # 输出数据文件（运行转换后生成）
└── temp/           # 临时数据文件
```

## 📄 输入数据文件

### employees.csv
员工信息表
- **字段**：id, name, department, salary, hire_date, email
- **用途**：员工数据相关练习
- **记录数**：10条

### products.csv
产品信息表
- **字段**：product_id, product_name, category, price, stock, create_date
- **用途**：产品数据相关练习
- **记录数**：10条

### sales.csv
销售记录表
- **字段**：sale_id, product_id, employee_id, quantity, amount, sale_date
- **用途**：销售数据分析练习
- **记录数**：15条

### departments.json
部门信息表（JSON格式）
- **字段**：dept_id, dept_name, manager, budget, location
- **用途**：学习JSON文件输入
- **记录数**：4条

### customers.txt
客户信息表（文本格式，分隔符：|）
- **字段**：customer_id, name, phone, address, level, register_date
- **用途**：学习文本文件输入
- **记录数**：10条

### orders.csv
订单信息表
- **字段**：order_id, customer_id, product_id, quantity, order_date, status
- **用途**：订单数据分析练习
- **记录数**：10条

### salary_grades.txt
薪资等级表（文本格式，分隔符：|）
- **字段**：grade, min_salary, max_salary, bonus_rate
- **用途**：薪资等级匹配练习
- **记录数**：4条

## 🔗 数据关系

```
employees (员工)
  ├── id -> sales.employee_id
  └── department -> departments.dept_name

products (产品)
  ├── product_id -> sales.product_id
  └── product_id -> orders.product_id

sales (销售)
  ├── employee_id -> employees.id
  └── product_id -> products.product_id

orders (订单)
  ├── customer_id -> customers.customer_id
  └── product_id -> products.product_id

customers (客户)
  └── customer_id -> orders.customer_id

departments (部门)
  └── dept_name -> employees.department
```

## 💡 使用建议

1. **不要修改原始数据文件**，保持数据一致性
2. **输出文件会自动生成**在output目录
3. **可以复制数据文件**用于测试和实验
4. **临时文件**可以放在temp目录

## 📊 数据统计

- 员工总数：10人
- 产品总数：10个
- 销售记录：15条
- 订单记录：10条
- 客户总数：10人
- 部门总数：4个

## 🔄 数据更新

如果需要更多测试数据：
1. 可以手动添加数据行
2. 保持字段格式一致
3. 注意数据关联关系
4. 建议备份原始文件
