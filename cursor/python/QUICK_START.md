# Python 快速开始指南

## 🚀 第一步：环境准备

### 1. 检查Python安装

打开命令行（Windows: PowerShell 或 CMD），输入：

```bash
python --version
```

如果显示版本号（如 Python 3.9.0），说明已安装。如果没有，请访问 [Python官网](https://www.python.org/downloads/) 下载安装。

### 2. 安装必要的库（可选）

对于数据处理练习，需要安装：

```bash
pip install numpy pandas matplotlib seaborn
```

## 📋 第二步：了解目录结构

```
python/
├── 知识点总结.md          # 完整知识点参考
├── exercises/            # 练习案例
│   ├── 01_基础语法练习.md
│   ├── 02_函数和模块练习.md
│   ├── 03_面向对象编程练习.md
│   ├── 04_文件操作练习.md
│   └── 05_数据处理练习.md
└── data/                 # 练习数据
    ├── scores.csv
    ├── students.csv
    └── ... (更多数据文件)
```

## 🎯 第三步：完成第一个练习

### 练习1：Hello World 和变量

1. **创建Python文件**

   在 `python/` 目录下创建 `hello.py`：

   ```python
   # hello.py
   print("Hello, Python!")
   
   # 定义变量
   name = "张三"
   age = 25
   height = 175.5
   
   # 打印变量
   print(f"姓名: {name}")
   print(f"年龄: {age}")
   print(f"身高: {height}cm")
   ```

2. **运行程序**

   在命令行中：

   ```bash
   cd python
   python hello.py
   ```

3. **查看结果**

   应该看到输出：
   ```
   Hello, Python!
   姓名: 张三
   年龄: 25
   身高: 175.5cm
   ```

### 练习2：读取CSV文件

1. **创建脚本 `read_csv.py`**：

   ```python
   # read_csv.py
   import csv
   
   # 读取CSV文件
   with open('data/scores.csv', 'r', encoding='utf-8') as f:
       reader = csv.DictReader(f)
       for row in reader:
           name = row['姓名']
           score = int(row['成绩'])
           print(f"{name}: {score}分")
   ```

2. **运行脚本**：

   ```bash
   python read_csv.py
   ```

3. **查看结果**

   应该看到所有学生的成绩。

## 📚 第四步：按顺序学习

### 推荐学习顺序

1. **第1周：基础语法**
   - 完成 `exercises/01_基础语法练习.md` 中的练习1-5
   - 重点：变量、数据类型、控制结构

2. **第2周：继续基础**
   - 完成 `exercises/01_基础语法练习.md` 中的练习6-10
   - 重点：列表、字典、集合操作

3. **第3周：函数和模块**
   - 完成 `exercises/02_函数和模块练习.md`
   - 重点：函数定义、参数、模块导入

4. **第4周：面向对象**
   - 完成 `exercises/03_面向对象编程练习.md`
   - 重点：类、对象、继承

5. **第5周：文件操作**
   - 完成 `exercises/04_文件操作练习.md`
   - 重点：文件读写、CSV/JSON处理

6. **第6周：数据处理**
   - 完成 `exercises/05_数据处理练习.md`
   - 重点：NumPy、Pandas、数据可视化

## 💡 学习技巧

### 1. 理解代码

不要只是复制粘贴，要理解每一行代码的作用。

### 2. 修改代码

尝试修改代码，看看会发生什么：
- 改变变量值
- 修改条件判断
- 添加新功能

### 3. 调试技巧

遇到错误时：
1. 仔细阅读错误信息
2. 检查代码语法
3. 使用 `print()` 输出中间值
4. 查看数据文件是否正确

### 4. 查阅文档

遇到不熟悉的函数：
- 使用 `help()` 函数：`help(print)`
- 查看官方文档
- 搜索在线资源

## 🔍 常用命令

### 运行Python脚本
```bash
python script.py
```

### 进入Python交互模式
```bash
python
```

### 安装库
```bash
pip install 库名
```

### 查看已安装的库
```bash
pip list
```

## 📝 创建输出目录

建议创建 `output/` 目录存放练习输出：

```bash
mkdir output
```

## ❓ 遇到问题？

### 常见错误

1. **文件找不到**
   - 检查文件路径是否正确
   - 确保在正确的目录下运行脚本

2. **编码错误**
   - 在文件开头添加：`# -*- coding: utf-8 -*-`
   - 打开文件时指定编码：`open('file.csv', encoding='utf-8')`

3. **语法错误**
   - 检查缩进（Python使用4个空格）
   - 检查括号是否匹配
   - 检查引号是否匹配

### 获取帮助

- 查看练习文档中的说明
- 查看 `知识点总结.md`
- 搜索Python官方文档
- 在Python交互模式中使用 `help()`

## 🎓 下一步

1. ✅ 完成环境准备
2. ✅ 运行第一个程序
3. 📖 阅读 `知识点总结.md`
4. 📝 开始完成练习
5. 💪 坚持每天练习

## 📖 推荐资源

- **官方文档**: https://docs.python.org/zh-cn/3/
- **Python教程**: https://docs.python.org/zh-cn/3/tutorial/
- **在线练习**: https://www.hackerrank.com/domains/python

---

**开始你的Python学习之旅吧！** 🚀

记住：编程是一门实践性很强的技能，多写代码才能提高！
