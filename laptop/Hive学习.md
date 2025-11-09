## 1.hadoop访问页面

（4）Web端查看HDFS的NameNode

①浏览器中输入：http://hadoop102:9870

​              ②查看HDFS上存储的数据信息

（5）Web端查看YARN的ResourceManager

①浏览器中输入：http://hadoop103:8088

​       ②查看YARN上运行的Job信息

## 2.Hive 分区表案例

案例原始表

```
create table t_all_hero(
    id int,
    name string,
    hp_max int,
    mp_max int,
    attack_max int,
    defense_max int,
    attack_range string,
    role_main string,
    role_assist string
)
row format delimited
fields terminated by "\t";
```

写入数据

```
hadoop fs -put /home/atguigu/data/hero/* /user/hive/warehouse/itheima.db/t_all_hero/
```



### 2.1 静态分区

1.建表语句

```
create table t_all_hero_part(
       id int,
       name string,
       hp_max int,
       mp_max int,
       attack_max int,
       defense_max int,
       attack_range string,
       role_main string,
       role_assist string
) partitioned by (role string)
row format delimited
fields terminated by "\t";
```

2.导入数据

```
load data local inpath '/home/atguigu/data/hero/archer.txt' into table t_all_hero_part partition(role='sheshou');
```

3.查询指定分区中的数据

```
select * from t_all_hero_part where role = 'sheshou';
```

### 2.2 动态分区

所谓`动态分区`指的是分区的字段值是基于查询结果自动推断出来的。核心语法就是`insert+select`。

启用hive动态分区，需要在hive会话中设置两个参数：

```powershell
set hive.exec.dynamic.partition=true;
set hive.exec.dynamic.partition.mode=nonstrict;
```

第一个参数表示开启动态分区功能，第二个参数指定动态分区的模式。分为nonstick非严格模式和strict严格模式。`strict严格模式要求至少有一个分区为静态分区`

1.建表语句

```
create table t_all_hero_part_dynamic(
         id int,
         name string,
         hp_max int,
         mp_max int,
         attack_max int,
         defense_max int,
         attack_range string,
         role_main string,
         role_assist string
) partitioned by (role string)
row format delimited
fields terminated by "\t";
```

2.插入数据

```
insert into table t_all_hero_part_dynamic partition(role) select tmp.*,tmp.role_main from t_all_hero tmp;
```

3.分区表使用

```
--非分区表 全表扫描过滤查询
select count(*) from t_all_hero where role_main="archer" and hp_max > 6000;

--分区表 先基于分区过滤 再查询
select count(*) from t_all_hero_part where role="archer" and hp_max > 6000;
```

2.3 多重分区表

建表语句

```
--单分区表，按省份分区
create table t_user_province (id int, name string,age int) partitioned by (province string);

--双分区表，按省份和市分区
create table t_user_province_city (id int, name string,age int) partitioned by (province string, city string);

--三分区表，按省份、市、县分区
create table t_user_province_city_county (id int, name string,age int) partitioned by (province string, city string, county string);
```

插入数据

```
load data local inpath '文件路径' into table t_user_province partition(province='shanghai');

load data local inpath '文件路径' into table t_user_province_city_county partition(province='zhejiang',city='hangzhou',county='xiaoshan');

select * from t_user_province_city_county where province='zhejiang' and city='hangzhou' and country='xiaoshan';
```

## 3.分桶表案例

1.建表语句

```
CREATE TABLE t_usa_covid19_bucket(
    count_date string,
    county string,
    state string,
    fips int,
    cases int,
    deaths int)
CLUSTERED BY(state) INTO 5 BUCKETS;
```

在创建分桶表时，还可以指定分桶内的数据排序规则

```
--根据state州分为5桶 每个桶内根据cases确诊病例数倒序排序
CREATE TABLE t_usa_covid19_bucket_sort(
      count_date string,
      county string,
      state string,
      fips int,
      cases int,
      deaths int)
CLUSTERED BY(state) sorted by (cases desc) INTO 5 BUCKETS;
```

2.分桶表数据加载

注意：分桶表数据的加载只能通过insert + select方式实现不能通过load data方式加载！

```
--step1:开启分桶的功能 从Hive2.0开始不再需要设置
set hive.enforce.bucketing=true;

--step2:把源数据加载到普通hive表中
CREATE TABLE  t_usa_covid19(
       count_date string,
       county string,
       state string,
       fips int,
       cases int,
       deaths int)
row format delimited fields terminated by ",";

--将源数据上传到HDFS，t_usa_covid19表对应的路径下
hadoop fs -put /home/atguigu/data/us-covid19-counties.dat /user/hive/warehouse/itheima.db/t_usa_covid19

--step3:使用insert+select语法将数据加载到分桶表中
insert into t_usa_covid19_bucket select * from t_usa_covid19 CLUSTER BY(state);
insert into t_usa_covid19_bucket_sort select * from t_usa_covid19 CLUSTER BY(state);
```

## 4.hive导出数据

###  insert + directory导出数据

Hive支持将select查询的结果导出成文件存放在文件系统中。语法格式如下：

```sql
--标准语法:
INSERT OVERWRITE [LOCAL] DIRECTORY directory1
    [ROW FORMAT row_format] [STORED AS file_format] (Note: Only available starting with Hive 0.11.0)
SELECT ... FROM ...

--Hive extension (multiple inserts):
FROM from_statement
INSERT OVERWRITE [LOCAL] DIRECTORY directory1 select_statement1
[INSERT OVERWRITE [LOCAL] DIRECTORY directory2 select_statement2] ...

--row_format
: DELIMITED [FIELDS TERMINATED BY char [ESCAPED BY char]] [COLLECTION ITEMS TERMINATED BY char]
[MAP KEYS TERMINATED BY char] [LINES TERMINATED BY char]
```

注意，==导出操作是一个OVERWRITE覆盖操作。慎重。==

目录可以是完整的URI。如果未指定scheme或Authority，则Hive将使用hadoop配置变量fs.default.name中的方案和Authority，该变量指定Namenode URI。

如果使用LOCAL关键字，则Hive会将数据写入本地文件系统上的目录。

写入文件系统的数据被序列化为文本，列之间用^A隔开，行之间用换行符隔开。如果任何列都不是原始类型，那么这些列将序列化为JSON格式。也可以在导出的时候指定分隔符换行符和文件格式。

```
--当前库下已有一张表student
select * from t_all_hero;

--1、导出查询结果到HDFS指定目录下
insert overwrite directory '/tmp/hive_export/e1' select * from student;

--2、导出时指定分隔符和文件存储格式
insert overwrite directory '/tmp/hive_export/e2' 
row format delimited fields terminated by ','
stored as orc
select * from student;

--3、导出数据到本地文件系统指定目录下
insert overwrite local directory '/home/atguigu/hive_export/e1' select * from t_all_hero;

如果没有指定stored as 其他存储类型，默认采用textfile类型（行存储）
```

## 5.hive查询

**UDF**（User-Defined-Function）普通函数，一进一出

**UDAF**（User-Defined Aggregation Function）聚合函数，多进一出

**UDTF**（User-Defined Table-Generating Functions）表生成函数，一进多出

如何判断一个函数属于UDF、UDAF还是UDTF呢？

答：`describe function extended 函数名称`

## 6.窗口函数

1.建表语句

```
create table website_pv_info(
   cookieid string,
   createtime string,   --day
   pv int
) row format delimited
fields terminated by ',';

create table website_url_info (
    cookieid string,
    createtime string,  --访问时间
    url string          --访问页面
) row format delimited
fields terminated by ',';
```

2.插入数据并查看

```
load data local inpath '/home/atguigu/data/website_pv_info.txt' into table website_pv_info;
load data local inpath '/home/atguigu/data/website_url_info.txt' into table website_url_info;

select * from website_pv_info;
select * from website_url_info;
```

3.窗口案例

```
--需求：求出每个用户总pv数
--sum(...) over( partition by... )，同组内所行求和
select cookieid,createtime,pv,
       sum(pv) over(partition by cookieid) as total_pv
from website_pv_info;
---  省略 rows between  默认则是 rows between unbounded preceding and unbounded following

cookie1 2018-04-10      1       26
cookie1 2018-04-16      4       26
cookie1 2018-04-15      4       26
cookie1 2018-04-14      2       26
cookie1 2018-04-13      3       26
cookie1 2018-04-12      7       26
cookie1 2018-04-11      5       26
cookie2 2018-04-16      7       35
cookie2 2018-04-15      9       35
cookie2 2018-04-14      3       35
cookie2 2018-04-13      6       35
cookie2 2018-04-12      5       35
cookie2 2018-04-11      3       35
cookie2 2018-04-10      2       35

--需求：求出每个用户截止到当天，累积的总pv数
--sum(...) over( partition by... order by ... )，在每个分组内，连续累积求和
select cookieid,createtime,pv,
       sum(pv) over(partition by cookieid order by createtime) as current_total_pv
from website_pv_info;

cookie1 2018-04-10      1       1
cookie1 2018-04-11      5       6
cookie1 2018-04-12      7       13
cookie1 2018-04-13      3       16
cookie1 2018-04-14      2       18
cookie1 2018-04-15      4       22
cookie1 2018-04-16      4       26
cookie2 2018-04-10      2       2
cookie2 2018-04-11      3       5
cookie2 2018-04-12      5       10
cookie2 2018-04-13      6       16
cookie2 2018-04-14      3       19
cookie2 2018-04-15      9       28
cookie2 2018-04-16      7       35

```

### 6.1rows between语法

```
关键字是rows between，包括下面这几个选项
- preceding：往前
- following：往后
- current row：当前行
- unbounded：边界
- unbounded preceding 表示从前面的起点
- unbounded following：表示到后面的终点
```

```
---窗口表达式
--第一行到当前行
select cookieid,createtime,pv,
       sum(pv) over(partition by cookieid order by createtime rows between unbounded preceding and current row) as pv2
from website_pv_info;

--向前3行至当前行
select cookieid,createtime,pv,
       sum(pv) over(partition by cookieid order by createtime rows between 3 preceding and current row) as pv4
from website_pv_info;

--向前3行 向后1行
select cookieid,createtime,pv,
       sum(pv) over(partition by cookieid order by createtime rows between 3 preceding and 1 following) as pv5
from website_pv_info;

--当前行至最后一行
select cookieid,createtime,pv,
       sum(pv) over(partition by cookieid order by createtime rows between current row and unbounded following) as pv6
from website_pv_info;

--第一行到最后一行 也就是分组内的所有行
select cookieid,createtime,pv,
       sum(pv) over(partition by cookieid order by createtime rows between unbounded preceding and unbounded following) as pv6
from website_pv_info;
```

### 6.2 rows between与range between

1.建表语句

```
create table t_rows_range(
	 id int
) row format delimited
fields terminated by ',';

load data local inpath '/home/atguigu/data/data.txt' into table t_rows_range;
```

2.查询语句

```
select
    id,
    sum(id) over(order by id rows between 1 preceding and 1 following) as rows_sum,
    sum(id) over(order by id range between 1 preceding and 1 following) as range_sum
from t_rows_range;

1       4       1
3       9       3
5       15      5
7       21      7
9       16      9
```

### 6.3 排名函数（重点 => 求TopN）

`聚合函数()/排名函数() + over()`

==row_number()==：在每个分组中，为每行分配一个从1开始的唯一序列号，递增，不考虑重复；

==rank()==: 在每个分组中，为每行分配一个从1开始的序列号，考虑重复，挤占后续位置；

==dense_rank()==: 在每个分组中，为每行分配一个从1开始的序列号，考虑重复，不挤占后续位置；



​                 rank    dense_rank   row_number

小明   90     1               1                     1

小丽   90     1               1                     2

小红   80     3               2                     3

rank() over()  ：有并列但是编号不连续

dense_rank() over() ：有并列且编号连续

row_number() over() ：编号连续但是没有并列情况（行号）

```
--需求：找出每个用户访问pv最多的Top3 重复并列的不考虑
SELECT * from
(SELECT
    cookieid,
    createtime,
    pv,
    ROW_NUMBER() OVER(PARTITION BY cookieid ORDER BY pv DESC) AS seq
FROM website_pv_info) tmp where tmp.seq < 4;
```

### 6.4 NTILE分析函数（分成几份）

​							评分

广东 深圳  店铺1  9   1

广东 深圳  店铺2  8   2

​												NTILE(2) OVER(partition by city order by rating desc) 

广东 深圳  店铺3  7   3

广东 深圳  店铺4  6   4



分析函数() + over()

```
--把每个分组内的数据分为3桶
SELECT
    cookieid,
    createtime,
    pv,
    NTILE(3) OVER(PARTITION BY cookieid ORDER BY createtime) AS rn2
FROM website_pv_info
ORDER BY cookieid,createtime;

注意：
partition by中的order by排序：相当于组内排序
最后这个order by：相当于全局排序（针对产生的结果，最终排序）

6  分成  3份，每份2条记录
7  分成  3份，第1份有3条记录，其他两份都是2条记录
8  分成  3份，第1份、2份有3条记录，另外一份有2条记录
```

### 6.5 窗口分析函数

1  LAG		2  当前行	  3  LEAD

==LAG(col,n,DEFAULT)==用于统计窗口内往上第n行值（滞后）

第一个参数为列名，第二个参数为往上第n行（可选，默认为1），第三个参数为默认值（当往上第n行为NULL时候，取默认值，如不指定，则为NULL）；

==LEAD(col,n,DEFAULT)== 用于统计窗口内往下第n行值（领先）

第一个参数为列名，第二个参数为往下第n行（可选，默认为1），第三个参数为默认值（当往下第n行为NULL时候，取默认值，如不指定，则为NULL）；

==FIRST_VALUE== 取分组内排序后，截止到当前行，第一个值；

==LAST_VALUE== 取分组内排序后，截止到当前行，最后一个值 => 容易踩坑；

```
-----------窗口分析函数----------
--LAG
SELECT cookieid,
       createtime,
       url,
       ROW_NUMBER() OVER(PARTITION BY cookieid ORDER BY createtime) AS rn,
       LAG(createtime,1,'1970-01-01 00:00:00') OVER(PARTITION BY cookieid ORDER BY createtime) AS last_1_time,
       LAG(createtime,2) OVER(PARTITION BY cookieid ORDER BY createtime) AS last_2_time
FROM website_url_info;

SELECT cookieid,
       createtime,
       url,
       ROW_NUMBER() OVER(PARTITION BY cookieid ORDER BY createtime) AS rn,
       LAG(createtime,1) OVER(PARTITION BY cookieid ORDER BY createtime) AS last_1_time,
       LAG(createtime,2) OVER(PARTITION BY cookieid ORDER BY createtime) AS last_2_time
FROM website_url_info;

--LEAD
SELECT cookieid,
       createtime,
       url,
       ROW_NUMBER() OVER(PARTITION BY cookieid ORDER BY createtime) AS rn,
       LEAD(createtime,1,'1970-01-01 00:00:00') OVER(PARTITION BY cookieid ORDER BY createtime) AS next_1_time,
       LEAD(createtime,2) OVER(PARTITION BY cookieid ORDER BY createtime) AS next_2_time
FROM website_url_info;

--FIRST_VALUE
SELECT cookieid,
       createtime,
       url,
       ROW_NUMBER() OVER(PARTITION BY cookieid ORDER BY createtime) AS rn,
       FIRST_VALUE(url) OVER(PARTITION BY cookieid ORDER BY createtime) AS first1
FROM website_url_info;

--LAST_VALUE
SELECT cookieid,
       createtime,
       url,
       ROW_NUMBER() OVER(PARTITION BY cookieid ORDER BY createtime) AS rn,
       LAST_VALUE(url) OVER(PARTITION BY cookieid ORDER BY createtime rows between unbounded preceding and unbounded following) AS last1
FROM website_url_info;
```

很多小伙伴，对LEAD和LAG感觉比较简单，但是不知道在实际工作中有何用途。

强调一下：在实际工作中，指标运算 => 同比或者环比通常都是通过LEAD或者LAG来进行实现。

同比：今年与去年相同日期比较，今年2024-05-01出行人数 与 2023-05-01进行比较

环比：今天和昨天日期相比