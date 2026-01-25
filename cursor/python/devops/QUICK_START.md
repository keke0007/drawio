# Python 自动化运维 - 快速开始指南

## 🚀 第一步：环境准备

### 1. 检查Python安装

打开命令行，输入：

```bash
python --version
```

如果显示版本号（如 Python 3.9.0），说明已安装。如果没有，请访问 [Python官网](https://www.python.org/downloads/) 下载安装。

### 2. 安装必要的库

```bash
pip install psutil paramiko requests pandas matplotlib watchdog
```

主要库说明：
- **psutil**: 系统和进程工具（必需）
- **paramiko**: SSH客户端（远程部署需要）
- **requests**: HTTP库（API调用）
- **pandas**: 数据处理（数据分析）
- **matplotlib**: 数据可视化（图表生成）
- **watchdog**: 文件监控（文件监控需要）

## 📋 第二步：了解目录结构

```
devops/
├── 知识点总结.md          # 完整知识点参考
├── exercises/            # 练习案例
│   ├── 01_系统监控练习.md
│   ├── 02_日志管理练习.md
│   ├── 03_文件管理练习.md
│   ├── 04_进程管理练习.md
│   └── 05_自动化部署练习.md
├── data/                 # 练习数据
│   ├── access.log
│   ├── error.log
│   └── ... (更多数据文件)
└── configs/              # 配置文件
    ├── monitor_config.json
    └── ... (更多配置文件)
```

## 🎯 第三步：完成第一个练习

### 练习1：获取系统信息

1. **创建Python文件 `system_info.py`**：

   ```python
   # system_info.py
   import psutil
   import json
   from datetime import datetime
   
   def get_system_info():
       """获取系统信息"""
       info = {
           "timestamp": datetime.now().isoformat(),
           "cpu": {
               "count": psutil.cpu_count(),
               "percent": psutil.cpu_percent(interval=1),
               "freq": psutil.cpu_freq()._asdict() if psutil.cpu_freq() else None
           },
           "memory": {
               "total": psutil.virtual_memory().total,
               "available": psutil.virtual_memory().available,
               "percent": psutil.virtual_memory().percent,
               "used": psutil.virtual_memory().used
           },
           "disk": {
               "total": psutil.disk_usage('/').total,
               "used": psutil.disk_usage('/').used,
               "free": psutil.disk_usage('/').free,
               "percent": psutil.disk_usage('/').percent
           }
       }
       return info
   
   if __name__ == "__main__":
       info = get_system_info()
       print(json.dumps(info, indent=2))
   ```

2. **运行脚本**：

   ```bash
   cd devops
   python system_info.py
   ```

3. **查看结果**

   应该看到系统信息的JSON输出。

### 练习2：解析访问日志

1. **创建脚本 `parse_log.py`**：

   ```python
   # parse_log.py
   import re
   from collections import Counter
   
   def parse_access_log(filename):
       """解析访问日志"""
       log_pattern = r'(\S+) - - \[(.*?)\] "(\S+) (\S+) (\S+)" (\d+) (\d+)'
       stats = {
           'total': 0,
           'status_codes': Counter(),
           'ips': Counter(),
           'methods': Counter()
       }
       
       with open(filename, 'r', encoding='utf-8') as f:
           for line in f:
               match = re.match(log_pattern, line)
               if match:
                   ip, time, method, path, protocol, status, size = match.groups()
                   stats['total'] += 1
                   stats['status_codes'][status] += 1
                   stats['ips'][ip] += 1
                   stats['methods'][method] += 1
       
       return stats
   
   if __name__ == "__main__":
       stats = parse_access_log('data/access.log')
       print(f"总访问数: {stats['total']}")
       print(f"状态码分布: {dict(stats['status_codes'])}")
       print(f"访问最多的IP: {stats['ips'].most_common(3)}")
   ```

2. **运行脚本**：

   ```bash
   python parse_log.py
   ```

## 📚 第四步：按顺序学习

### 推荐学习顺序

1. **第1周：系统监控**
   - 完成 `exercises/01_系统监控练习.md` 中的练习1-5
   - 重点：系统信息获取、资源监控

2. **第2周：日志管理**
   - 完成 `exercises/02_日志管理练习.md`
   - 重点：日志解析、日志分析

3. **第3周：文件管理**
   - 完成 `exercises/03_文件管理练习.md`
   - 重点：文件操作、文件监控

4. **第4周：进程管理**
   - 完成 `exercises/04_进程管理练习.md`
   - 重点：进程监控、服务管理

5. **第5周：自动化部署**
   - 完成 `exercises/05_自动化部署练习.md`
   - 重点：远程部署、CI/CD

## 💡 学习技巧

### 1. 理解代码

不要只是复制粘贴，要理解每一行代码的作用。

### 2. 修改代码

尝试修改代码，看看会发生什么：
- 改变监控间隔
- 修改告警阈值
- 添加新功能

### 3. 调试技巧

遇到错误时：
1. 仔细阅读错误信息
2. 检查代码语法
3. 使用 `print()` 输出中间值
4. 查看数据文件是否正确

### 4. 查阅文档

遇到不熟悉的函数：
- 使用 `help()` 函数：`help(psutil.cpu_percent)`
- 查看库的官方文档
- 搜索在线资源

## 🔍 常用命令

### 运行Python脚本
```bash
python script.py
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

1. **模块未找到**
   - 检查是否安装了所需的库
   - 使用 `pip install 库名` 安装

2. **文件找不到**
   - 检查文件路径是否正确
   - 确保在正确的目录下运行脚本

3. **权限错误**
   - 检查文件权限
   - 某些操作需要管理员权限

### 获取帮助

- 查看练习文档中的说明
- 查看 `知识点总结.md`
- 搜索库的官方文档
- 在Python交互模式中使用 `help()`

## 🎓 下一步

1. ✅ 完成环境准备
2. ✅ 运行第一个程序
3. 📖 阅读 `知识点总结.md`
4. 📝 开始完成练习
5. 💪 坚持每天练习

## 📖 推荐资源

- **psutil文档**: https://psutil.readthedocs.io/
- **paramiko文档**: http://www.paramiko.org/
- **pandas文档**: https://pandas.pydata.org/docs/
- **Ansible文档**: https://docs.ansible.com/

---

**开始你的自动化运维学习之旅吧！** 🚀

记住：自动化运维需要大量的实践，多写脚本才能提高！
