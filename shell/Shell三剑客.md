## 1. 简单题：Shell三剑客分别是什么？它们各自的主要作用是什么？

```powershell

```

## 2.简答题：请列举出grep命令中常用的几个选项及作用

基本语法：`grep [OPTION]... PATTERN [FILE]...`

| 支持的正则           | 描述 |
| -------------------- | ---- |
| -E,--extended-regexp |      |
| -P,--perl-regexp     |      |
| -i,--ignore-case     |      |
| -w,--word-regexp     |      |
| -v，--invert-match   |      |

| 输出控制           | 描述 |
| ------------------ | ---- |
| -n,--line-number   |      |
| -o,--only-matching |      |
| -r,--recursive     |      |
| -c,--count         |      |

## 3.简答题：sed流程是怎样的？

```powershell
```

## 4.awk命令的模式与动作分别是什么？它们在awk什么角色？

```powershell
```



## 5. 编程题一：

需求：编写一个`grep`命令，用于在当前目录（/root）及其子目录下的所有`.txt`文件中搜索包含单词"example"的行，并忽略大小写。

```powershell
```

## 6. 编程题二：

需求：使用`sed`命令将文件`person.txt`中所有的"Tom"替换为"Eric"，同时对原文件进行备份为person.txt.bak

```powershell

```

## 7. 编程题三：

需求：编写一个`awk`命令，用于从文件`/etc/passwd`中提取出所有用户的用户名和对应的用户ID并输出

```powershell
```

## 8. 编程题四：

数据文件

```powershell
vim example.txt
101,John,25,Engineer
102,Jane,30,Doctor
103,Bob,28,Teacher
104,Alice,32,Manager
105,Chris,29,Designer
```

需求1：在第3行后追加一行文本

```powershell
```

需求2：在第2行前插入一行文本

```powershell
```

需求3：删除第4行

```powershell
```

需求4：删除包含"Doctor"的所有行

```powershell
```

需求5：将第2行的"Jane"替换为"Jill"

```powershell
```

需求6：将所有"Engineer"替换为"Developer"

```powershell
```

需求7：打印第3行

```powershell
```

## 9. 编程题五：

数据文件

```powershell
vim example.txt
ID,Name,Age,Occupation
101,John,25,Engineer
102,Jane,30,Doctor
103,Bob,28,Teacher
104,Alice,32,Manager
105,Chris,29,Designer
```

需求1：提取文件中所有记录的ID和Occupation列

```powershell
```

需求2：提取文件中的第2行~第4行

```powershell
```

需求3：提取文件中，3-5行中的Name和Age列

```powershell
```

需求4：提取文件中Name和Occupation列，并显示行号信息

```powershell
```

需求5：设定行与行之间的分隔符为","然后显示每行内容

```powershell
```

