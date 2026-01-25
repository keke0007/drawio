# Python 自动化运维学习指南

欢迎来到Python自动化运维学习目录！这里包含了系统的自动化运维知识点、练习案例和配套数据。

## 📁 目录结构

```
devops/
├── 知识点总结.md          # 自动化运维完整知识点总结
├── exercises/            # 练习案例目录
│   ├── 01_系统监控练习.md
│   ├── 02_日志管理练习.md
│   ├── 03_文件管理练习.md
│   ├── 04_进程管理练习.md
│   └── 05_自动化部署练习.md
├── data/                 # 练习数据目录
│   ├── access.log        # Web访问日志
│   ├── error.log         # 错误日志
│   ├── app.log           # 应用日志
│   ├── nginx.log         # Nginx日志
│   ├── monitor_history.csv  # 监控历史数据
│   └── ... (更多数据文件)
├── configs/              # 配置文件目录
│   ├── monitor_config.json
│   ├── alert_rules.json
│   └── deploy_config.json
├── scripts/              # 示例脚本目录（可选）
└── README.md             # 本文件
```

## 🚀 快速开始

### 1. 环境准备

确保已安装Python 3.7或更高版本：

```bash
python --version
```

### 2. 安装依赖

安装自动化运维所需的库：

```bash
pip install psutil paramiko requests pandas matplotlib watchdog
```

主要库说明：
- **psutil**: 系统和进程工具
- **paramiko**: SSH客户端
- **requests**: HTTP库
- **pandas**: 数据处理
- **matplotlib**: 数据可视化
- **watchdog**: 文件监控

### 3. 学习路径

建议按照以下顺序学习：

1. **系统监控** (`exercises/01_系统监控练习.md`)
   - 系统信息获取
   - CPU、内存、磁盘监控
   - 监控告警

2. **日志管理** (`exercises/02_日志管理练习.md`)
   - 日志读取和解析
   - 日志分析和统计
   - 日志告警

3. **文件管理** (`exercises/03_文件管理练习.md`)
   - 文件批量操作
   - 文件监控和同步
   - 备份和归档

4. **进程管理** (`exercises/04_进程管理练习.md`)
   - 进程监控和控制
   - 服务管理
   - 进程守护化

5. **自动化部署** (`exercises/05_自动化部署练习.md`)
   - Git操作自动化
   - 远程部署
   - CI/CD流水线

## 📚 知识点概览

详细知识点请查看 [`知识点总结.md`](知识点总结.md)，包括：

- ✅ 系统监控
- ✅ 日志管理
- ✅ 文件操作
- ✅ 进程管理
- ✅ 网络管理
- ✅ 配置管理
- ✅ 自动化部署
- ✅ 备份与恢复
- ✅ 性能监控
- ✅ 服务管理
- ✅ 安全运维
- ✅ 容器化运维

## 💡 学习建议

### 每日学习计划

- **第1-2周**：完成系统监控和日志管理练习
- **第3-4周**：完成文件管理和进程管理练习
- **第5-6周**：完成自动化部署练习和综合项目

### 学习方法

1. **理论结合实践**
   - 先阅读知识点总结
   - 再完成对应练习
   - 最后尝试扩展功能

2. **循序渐进**
   - 从简单到复杂
   - 每个练习都要理解原理
   - 不要跳过基础练习

3. **多实践**
   - 修改练习代码
   - 尝试不同的实现方式
   - 解决实际问题

4. **做好笔记**
   - 记录重要概念
   - 记录遇到的问题
   - 记录解决方案

## 📝 练习说明

### 练习文件结构

每个练习文件包含：
- **任务描述**：要完成的功能
- **数据文件**：使用的数据文件路径
- **要求**：需要实现的功能点
- **示例代码**：部分示例代码（部分练习）

### 数据文件说明

所有数据文件位于 `data/` 目录：

- **日志文件**：用于日志管理练习
  - `access.log` - Web访问日志
  - `error.log` - 错误日志
  - `app.log` - 应用日志
  - `nginx.log` - Nginx访问日志

- **监控数据**：用于系统监控练习
  - `monitor_history.csv` - 监控历史数据
  - `memory_usage.log` - 内存使用日志

- **配置文件**：用于配置管理练习
  - `requirements.txt` - Python依赖

### 配置文件说明

配置文件位于 `configs/` 目录：

- `monitor_config.json` - 监控配置
- `alert_rules.json` - 告警规则
- `deploy_config.json` - 部署配置

### 输出目录

建议创建 `output/` 目录存放练习输出：

```bash
mkdir output
```

## 🔧 常用工具和库

### 系统监控
- **psutil**: 跨平台系统和进程工具
- **platform**: 平台信息
- **socket**: 网络接口

### 文件操作
- **os**: 操作系统接口
- **shutil**: 高级文件操作
- **pathlib**: 路径操作
- **watchdog**: 文件监控

### 网络操作
- **paramiko**: SSH客户端
- **requests**: HTTP库
- **urllib**: URL处理

### 数据处理
- **pandas**: 数据处理
- **numpy**: 数值计算
- **matplotlib**: 数据可视化

### 日志处理
- **logging**: 标准日志库
- **loguru**: 现代日志库

## 📖 学习资源

### 官方文档
- [psutil文档](https://psutil.readthedocs.io/)
- [paramiko文档](http://www.paramiko.org/)
- [pandas文档](https://pandas.pydata.org/docs/)

### 在线资源
- [Python自动化运维](https://www.python.org/)
- [Ansible文档](https://docs.ansible.com/)
- [Docker文档](https://docs.docker.com/)

### 实践平台
- 搭建本地测试环境
- 使用虚拟机练习
- 参与开源项目

## ❓ 常见问题

### Q: 如何获取系统信息？

A: 使用psutil库：
```python
import psutil
cpu_percent = psutil.cpu_percent(interval=1)
memory = psutil.virtual_memory()
```

### Q: 如何监控日志文件？

A: 使用watchdog库：
```python
from watchdog.observers import Observer
from watchdog.events import FileSystemEventHandler
```

### Q: 如何连接远程服务器？

A: 使用paramiko库：
```python
import paramiko
ssh = paramiko.SSHClient()
ssh.connect(hostname, username=user, key_filename=key_file)
```

### Q: 如何处理大日志文件？

A: 
1. 逐行读取，不一次性加载
2. 使用生成器
3. 分批处理
4. 使用数据库存储

## 🎯 学习目标

完成所有练习后，你应该能够：

- ✅ 监控系统资源（CPU、内存、磁盘、网络）
- ✅ 管理和分析日志文件
- ✅ 自动化文件操作和备份
- ✅ 管理进程和服务
- ✅ 实现自动化部署
- ✅ 编写运维脚本
- ✅ 构建监控告警系统

## 📈 进度跟踪

建议创建自己的学习进度文件，记录：
- 完成的练习
- 遇到的问题
- 学到的知识点
- 心得体会

## 🤝 贡献

如果你发现错误或有改进建议，欢迎：
- 修正数据文件
- 完善练习说明
- 添加新的练习

## ⚠️ 注意事项

### 安全提示
- 不要在生产环境直接运行未测试的脚本
- 保护好SSH密钥和密码
- 谨慎执行删除和修改操作
- 做好备份

### 最佳实践
- 使用配置文件管理参数
- 添加日志记录
- 实现错误处理
- 编写文档说明

## 📄 许可证

本学习材料仅供学习使用。

---

**祝学习顺利！** 🚀

如有问题，请查看对应的练习文档或参考相关库的官方文档。
