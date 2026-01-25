# DevOps 数据文件说明

本目录包含所有自动化运维练习所需的数据文件。

## 📁 数据文件列表

### 日志文件

- **access.log** - Web服务器访问日志（Apache/Nginx格式）
- **error.log** - 应用错误日志（包含ERROR、WARNING、CRITICAL级别）
- **app.log** - 应用运行日志（包含INFO、WARNING、ERROR级别）
- **nginx.log** - Nginx访问日志（完整格式）
- **combined.log** - 综合日志（包含多种日志级别）

### 监控数据

- **monitor_history.csv** - 系统监控历史数据（CPU、内存、磁盘、网络）
- **memory_usage.log** - 内存使用历史日志

### 服务日志

- **log1.log** - 服务1的日志
- **log2.log** - 服务2的日志
- **log3.log** - 服务3的日志

### 配置文件

- **requirements.txt** - Python项目依赖文件

## 📊 数据文件用途

### 系统监控练习
- `monitor_history.csv` - 监控数据分析和可视化
- `memory_usage.log` - 内存监控分析

### 日志管理练习
- `access.log` - Web访问日志分析
- `error.log` - 错误日志分析
- `app.log` - 应用日志实时监控
- `nginx.log` - Nginx日志解析和统计
- `combined.log` - 日志过滤和搜索
- `log1.log`, `log2.log`, `log3.log` - 日志聚合

### 文件管理练习
- 使用 `data/` 目录下的文件进行文件操作练习

### 进程管理练习
- 使用系统实时进程数据

### 自动化部署练习
- `requirements.txt` - 依赖管理

## 💡 使用说明

### 读取日志文件

```python
# 读取访问日志
with open('data/access.log', 'r', encoding='utf-8') as f:
    for line in f:
        # 解析日志行
        pass
```

### 读取CSV监控数据

```python
import pandas as pd

df = pd.read_csv('data/monitor_history.csv')
print(df.head())
```

### 解析日志格式

#### Apache/Nginx访问日志格式
```
IP - - [时间] "方法 路径 协议" 状态码 大小 "引用" "用户代理"
```

#### 应用日志格式
```
时间 级别 [模块:行号] 消息
```

## ⚠️ 注意事项

1. **文件编码**：所有文件使用UTF-8编码
2. **路径问题**：运行脚本时确保在正确的目录下
3. **数据格式**：日志文件遵循标准格式
4. **数据修改**：练习时可以修改数据，但建议保留原始数据备份

## 🔄 数据更新

如果需要添加新的数据文件：
1. 确保使用UTF-8编码
2. 遵循命名规范（小写字母、下划线）
3. 更新本README文件

---

**提示**：开始练习前，建议先查看对应的练习文档了解数据文件的用途。
