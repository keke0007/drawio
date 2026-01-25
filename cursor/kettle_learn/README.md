# Kettle (Pentaho Data Integration) 学习指南

## 📚 功能清单

### 一、数据输入（Input）
1. **表输入（Table Input）** - 从数据库读取数据
2. **CSV文件输入（CSV File Input）** - 读取CSV文件
3. **Excel输入（Excel Input）** - 读取Excel文件
4. **JSON输入（JSON Input）** - 读取JSON文件
5. **文本文件输入（Text File Input）** - 读取文本文件
6. **XML输入（XML Input）** - 读取XML文件
7. **生成记录（Generate Rows）** - 生成测试数据
8. **获取系统信息（Get System Info）** - 获取系统变量
9. **获取文件列表（Get File Names）** - 获取文件列表

### 二、数据输出（Output）
1. **表输出（Table Output）** - 写入数据库表
2. **插入/更新（Insert/Update）** - 插入或更新数据
3. **更新（Update）** - 更新数据库记录
4. **删除（Delete）** - 删除数据库记录
5. **CSV文件输出（CSV File Output）** - 输出到CSV文件
6. **Excel输出（Excel Output）** - 输出到Excel文件
7. **文本文件输出（Text File Output）** - 输出到文本文件
8. **JSON输出（JSON Output）** - 输出到JSON文件

### 三、数据转换（Transform）
1. **选择/过滤值（Select Values）** - 选择或重命名字段
2. **过滤记录（Filter Rows）** - 根据条件过滤数据
3. **排序记录（Sort Rows）** - 对数据进行排序
4. **分组（Group By）** - 数据分组聚合
5. **增加常量（Add Constants）** - 添加常量字段
6. **增加序列（Add Sequence）** - 添加序列号
7. **计算器（Calculator）** - 字段计算
8. **字符串操作（String Operations）** - 字符串处理
9. **值映射（Value Mapper）** - 值映射转换
10. **字段选择（Select Fields）** - 字段选择
11. **行转列（Row Denormaliser）** - 行转列
12. **列转行（Row Normaliser）** - 列转行
13. **拆分字段（Split Field）** - 拆分字段
14. **合并字段（Concat Fields）** - 合并字段
15. **替换NULL值（Replace NULL Value）** - 处理空值
16. **唯一行（Unique Rows）** - 去重
17. **记录去重（Deduplicate）** - 记录去重

### 四、数据连接（Join）
1. **流查询（Stream Lookup）** - 流查询
2. **数据库查询（Database Lookup）** - 数据库查询
3. **合并记录（Merge Rows）** - 合并记录
4. **合并连接（Merge Join）** - 合并连接
5. **记录集连接（Recordset Join）** - 记录集连接

### 五、数据验证（Validation）
1. **数据验证（Data Validator）** - 数据验证
2. **空操作（Dummy）** - 占位步骤
3. **中止（Abort）** - 中止转换

### 六、脚本（Scripting）
1. **JavaScript代码（JavaScript）** - JavaScript脚本
2. **Java代码（User Defined Java Class）** - Java类
3. **SQL脚本（Execute SQL Script）** - 执行SQL

### 七、流程控制（Flow）
1. **Switch/Case** - 条件分支
2. **空操作（Dummy）** - 占位步骤
3. **阻塞步骤（Blocking Step）** - 阻塞步骤
4. **延迟行（Delay Row）** - 延迟行

### 八、作业（Job）
1. **转换（Transformation）** - 执行转换
2. **作业（Job）** - 执行子作业
3. **成功（Success）** - 成功标记
4. **失败（Failure）** - 失败标记
5. **文件管理（File Management）** - 文件操作
6. **FTP（FTP）** - FTP操作
7. **邮件（Mail）** - 发送邮件
8. **定时（Timer）** - 定时执行

## 🎯 学习路径

### 阶段一：基础入门（1-3天）
- [x] 环境搭建
- [ ] 练习1：CSV文件读取和输出
- [ ] 练习2：数据库连接和查询
- [ ] 练习3：字段选择和重命名

### 阶段二：数据转换（4-7天）
- [ ] 练习4：数据过滤和排序
- [ ] 练习5：数据分组和聚合
- [ ] 练习6：字段计算和字符串处理
- [ ] 练习7：值映射和替换

### 阶段三：数据连接（8-10天）
- [ ] 练习8：表连接和查询
- [ ] 练习9：数据合并
- [ ] 练习10：数据去重

### 阶段四：高级功能（11-14天）
- [ ] 练习11：JavaScript脚本
- [ ] 练习12：条件分支
- [ ] 练习13：错误处理
- [ ] 练习14：作业调度

### 阶段五：参数传递（15-18天）
- [ ] 练习15：转换参数
- [ ] 练习16：作业参数
- [ ] 练习17：变量和系统变量
- [ ] 练习18：命名参数和参数集
- [ ] 练习19：命令行参数
- [ ] 练习20：参数验证和默认值
- [ ] 练习21：参数传递到子转换和子作业

### 阶段六：数组参数和循环操作（19-20天）
- [ ] 练习22：数组参数和循环删除
- [ ] 练习23：数组参数的其他应用场景

## 📁 项目结构

```
kettle_learn/
├── README.md                 # 本文件
├── data/                     # 示例数据目录
│   ├── input/               # 输入数据
│   ├── output/              # 输出数据
│   └── temp/                # 临时数据
├── transformations/          # 转换文件目录
│   ├── 01_basic/            # 基础转换
│   ├── 02_transform/        # 数据转换
│   ├── 03_join/             # 数据连接
│   └── 04_advanced/         # 高级功能
├── jobs/                     # 作业文件目录
└── docs/                     # 文档目录
    └── exercises/           # 练习案例说明
```

## 🚀 快速开始

1. 安装Kettle (Pentaho Data Integration)
2. 打开Spoon工具
3. 按照练习案例依次学习
4. 参考每个练习的说明文档

## 📝 练习案例

详细练习案例请查看 `docs/exercises/` 目录下的文档。
