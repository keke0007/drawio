# sSpark开发环境搭建

## 1.Spark基于Scala开发环境搭建

### 1.1 配置Hadoop环境变量

```
HADOOP_HOME
D:\develop\Bigdata\Hadoop\hadoop-3.1.0
```

### 1.2 配置Scala开发环境

```
SCALA_HOME
D:\develop\Scala\scala-2.12.15
```

### 1.3 IDEA下载Scala插件

```
Spark 的开发版本是 3.2.3
```



### 1.4 IDEA创建Maven项目运行SparkCore与SparkSQL

#### 1.4.1 运行SparkCore需要的Maven依赖

```xml
<properties>
        <maven.compiler.source>8</maven.compiler.source>
        <maven.compiler.target>8</maven.compiler.target>
        <encoding>UTF-8</encoding>
        <spark.version>3.2.3</spark.version>
        <scala.version>2.12.15</scala.version>
    </properties>

    <dependencies>
        <!-- scala的依赖 -->
        <dependency>
            <groupId>org.scala-lang</groupId>
            <artifactId>scala-library</artifactId>
            <version>${scala.version}</version>
            <scope>provided</scope>
        </dependency>

        <!-- spark core 即为spark内核 ，其他高级组件都要依赖spark core -->
        <dependency>
            <groupId>org.apache.spark</groupId>
            <artifactId>spark-core_2.12</artifactId>
            <version>${spark.version}</version>
            <!-- 编译时引入依赖，打包时不会引入该依赖 -->
<!--            <scope>provided</scope>-->
        </dependency>

        <dependency>
            <groupId>org.apache.spark</groupId>
            <artifactId>spark-sql_2.12</artifactId>
            <version>${spark.version}</version>
            <!-- 编译时引入依赖，打包时不会引入该依赖 -->
            <!--            <scope>provided</scope>-->
        </dependency>

        <dependency>
            <groupId>org.apache.spark</groupId>
            <artifactId>spark-streaming_2.12</artifactId>
            <version>${spark.version}</version>
            <!-- 编译时引入依赖，打包时不会引入该依赖 -->
            <!--            <scope>provided</scope>-->
        </dependency>

        <!-- https://mvnrepository.com/artifact/commons-lang/commons-lang -->
        <dependency>
            <groupId>commons-lang</groupId>
            <artifactId>commons-lang</artifactId>
            <version>2.6</version>
        </dependency>

        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.10</version>
            <scope>provided</scope>
        </dependency>

        <!-- log4j2 核心组件 -->
        <dependency>
            <groupId>org.apache.logging.log4j</groupId>
            <artifactId>log4j-api</artifactId>
            <version>2.17.2</version>
        </dependency>

        <dependency>
            <groupId>org.apache.logging.log4j</groupId>
            <artifactId>log4j-core</artifactId>
            <version>2.17.2</version>
        </dependency>

        <!-- Spark 3.2.x 兼容的 slf4j 桥接实现（基于 slf4j 1.7.x） -->
        <dependency>
            <groupId>org.apache.logging.log4j</groupId>
            <artifactId>log4j-slf4j-impl</artifactId>
            <version>2.17.2</version>
        </dependency>

        <!-- 让 log4j1 调用被转发到 log4j2 -->
        <dependency>
            <groupId>org.apache.logging.log4j</groupId>
            <artifactId>log4j-1.2-api</artifactId>
            <version>2.17.2</version>
        </dependency>

        <!-- 排除 Spark 自带旧版 log4j，以防冲突 -->
        <dependency>
            <groupId>org.apache.spark</groupId>
            <artifactId>spark-core_2.12</artifactId>
            <version>3.2.3</version>
            <exclusions>
                <exclusion>
                    <groupId>org.slf4j</groupId>
                    <artifactId>slf4j-log4j12</artifactId>
                </exclusion>
                <exclusion>
                    <groupId>log4j</groupId>
                    <artifactId>log4j</artifactId>
                </exclusion>
            </exclusions>
        </dependency>

    </dependencies>

    <!-- 配置Maven的镜像库 -->
    <!-- 依赖下载国内镜像库 -->
    <repositories>
        <repository>
            <id>nexus-aliyun</id>
            <name>Nexus aliyun</name>
            <layout>default</layout>
            <url>http://maven.aliyun.com/nexus/content/groups/public</url>
            <snapshots>
                <enabled>false</enabled>
                <updatePolicy>never</updatePolicy>
            </snapshots>
            <releases>
                <enabled>true</enabled>
                <updatePolicy>never</updatePolicy>
            </releases>
        </repository>
    </repositories>

    <!-- maven插件下载国内镜像库 -->
    <pluginRepositories>
        <pluginRepository>
            <id>ali-plugin</id>
            <url>http://maven.aliyun.com/nexus/content/groups/public/</url>
            <snapshots>
                <enabled>false</enabled>
                <updatePolicy>never</updatePolicy>
            </snapshots>
            <releases>
                <enabled>true</enabled>
                <updatePolicy>never</updatePolicy>
            </releases>
        </pluginRepository>
    </pluginRepositories>

    <build>
        <pluginManagement>
            <plugins>
                <!-- 编译scala的插件 -->
                <plugin>
                    <groupId>net.alchim31.maven</groupId>
                    <artifactId>scala-maven-plugin</artifactId>
                    <version>3.2.2</version>
                </plugin>
                <!-- 编译java的插件 -->
                <plugin>
                    <groupId>org.apache.maven.plugins</groupId>
                    <artifactId>maven-compiler-plugin</artifactId>
                    <version>3.5.1</version>
                </plugin>
            </plugins>
        </pluginManagement>
        <plugins>
            <plugin>
                <groupId>net.alchim31.maven</groupId>
                <artifactId>scala-maven-plugin</artifactId>
                <executions>
                    <execution>
                        <id>scala-compile-first</id>
                        <phase>process-resources</phase>
                        <goals>
                            <goal>add-source</goal>
                            <goal>compile</goal>
                        </goals>
                    </execution>
                    <execution>
                        <id>scala-test-compile</id>
                        <phase>process-test-resources</phase>
                        <goals>
                            <goal>testCompile</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>

            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <executions>
                    <execution>
                        <phase>compile</phase>
                        <goals>
                            <goal>compile</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>

            <!-- 打jar插件 -->
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-shade-plugin</artifactId>
                <version>2.4.3</version>
                <executions>
                    <execution>
                        <phase>package</phase>
                        <goals>
                            <goal>shade</goal>
                        </goals>
                        <configuration>
                            <filters>
                                <filter>
                                    <artifact>*:*</artifact>
                                    <excludes>
                                        <exclude>META-INF/*.SF</exclude>
                                        <exclude>META-INF/*.DSA</exclude>
                                        <exclude>META-INF/*.RSA</exclude>
                                    </excludes>
                                </filter>
                            </filters>
                        </configuration>
                    </execution>
                </executions>
            </plugin>
        </plugins>
    </build>
```

#### 1.4.2 配置log4j2.properties文件

```
# ============================
#  Spark ????????
# ============================

# ---- ?????? ----
appender.console.type = Console
appender.console.name = console
appender.console.layout.type = PatternLayout
appender.console.layout.pattern = [%d{yyyy-MM-dd HH:mm:ss}] [%-5p] [%c{1}] - %m%n

# ---- ??????????----
# INFO: ?????
# WARN: ????????????
# ERROR: ??????????
rootLogger.level = WARN
rootLogger.appenderRefs = console
rootLogger.appenderRef.console.ref = console

# ---- ?? Spark ???? ----
logger.spark.name = org.apache.spark
logger.spark.level = ERROR

logger.spark.storage.name = org.apache.spark.storage
logger.spark.storage.level = ERROR

logger.spark.scheduler.name = org.apache.spark.scheduler
logger.spark.scheduler.level = ERROR

logger.spark.executor.name = org.apache.spark.executor
logger.spark.executor.level = ERROR

logger.spark.sql.name = org.apache.spark.sql
logger.spark.sql.level = ERROR

logger.spark.repl.name = org.apache.spark.repl
logger.spark.repl.level = ERROR

# ---- ?? Akka / Jetty / Hadoop ----
logger.akka.name = akka
logger.akka.level = ERROR

logger.jetty.name = org.eclipse.jetty
logger.jetty.level = ERROR

logger.hadoop.name = org.apache.hadoop
logger.hadoop.level = ERROR

# ---- ??????????? ----
# ?? com.example ?????????
logger.app.name = com.example
logger.app.level = INFO
logger.app.additivity = false
logger.app.appenderRefs = console
logger.app.appenderRef.console.ref = console

# ---- Spark REPL ????????? ----
logger.spark.repl.Main.name = org.apache.spark.repl.Main
logger.spark.repl.Main.level = ERROR

# ---- ?? SparkContext ???????? ----
logger.sparkContext.name = org.apache.spark.SparkContext
logger.sparkContext.level = ERROR

```

#### 1.4.3 SparkCore运行Demo

```scala
 def main(args: Array[String]): Unit = {

    // 1. 初始化 Spark 配置与上下文
    val conf = new SparkConf()
      .setAppName("SparkMapCollectExample")
      .setMaster("local[*]") // 本地模式运行，可改成 Yarn 或 Standalone

    val sc = new SparkContext(conf)

    // 2. 创建一个集合数据源 (Source)
    val data = Seq(1, 2, 3, 4, 5)
    val rdd = sc.parallelize(data) // 将集合转换为 RDD

    // 3. 使用 map 算子进行转换操作
    val mappedRDD = rdd.map(x => x * x) // 将每个数字平方

    // 4. 使用 collect 将结果收集回 Driver 端
    val result = mappedRDD.collect()

    // 5. 打印结果
    println("原始数据: " + data.mkString(", "))
    println("平方结果: " + result.mkString(", "))

    // 6. 停止 SparkContext
    sc.stop()
  }
```

#### 1.4.4 SparkSql运行Demo

```scala
 @Test
  def dsIntro(): Unit = {
    val spark = SparkSession.builder()
      .appName("ds intro")
      .master("local[6]")
      .getOrCreate()

    val sourceRDD = spark.sparkContext.parallelize(Seq(Person("zs",18),Person("ls",8)))

    //import spark.implicits._ 是 Spark SQL 中非常重要的一句隐式转换导入语句，
    // 它为 DataFrame / Dataset 提供许多关键能力。下面用最清晰的方式说明它的作用。
    import spark.implicits._
    val personDS: Dataset[Person] = sourceRDD.toDS()

    val resultDS = personDS.where( 'age > 10 )
      .where( 'age < 20 )
      .select( 'name )
      .as[String]

    resultDS.show()

  }

  @Test
  def dfIntro(): Unit = {
    val spark = new SparkSession.Builder()
      .appName("ds intro")
      .master("local[6]")
      .getOrCreate()

    val sourceRDD = spark.sparkContext.parallelize(Seq(Person("zs",18),Person("ls",8)))

    //import spark.implicits._ 是 Spark SQL 中非常重要的一句隐式转换导入语句，
    // 它为 DataFrame / Dataset 提供许多关键能力。下面用最清晰的方式说明它的作用。
    import spark.implicits._
    val personDF = sourceRDD.toDF()

    personDF.createOrReplaceTempView("person")
    val resultDF = spark.sql("select name from person where age > 3 and age < 10")

    resultDF.show()

  }
  
  @Test
  def dfTestReadCsv(): Unit = {
    val spark = SparkSession.builder()
      .appName("ds intro")
      .master("local[6]")
      .getOrCreate()
    //import spark.implicits._ 是 Spark SQL 中非常重要的一句隐式转换导入语句，
    // 它为 DataFrame / Dataset 提供许多关键能力。下面用最清晰的方式说明它的作用。
    import spark.implicits._
    // 通过隐式转换创建的DataFrame
    val sourceDF = spark.read.option("header",value = true).csv("dataset/BeijingPM20100101_20151231.csv")

    /*sourceDF.printSchema()*/

    /*sourceDF.select('year,'month,'PM_Dongsi)
      .where('PM_Dongsi =!= "NA")
      .groupBy('year,'month)
      .count()
      .show()
*/
    // 使用sql查询
    sourceDF.createOrReplaceTempView("pm")

    spark.sql("select year, month, count(PM_Dongsi) from pm where PM_Dongsi != 'NA' group by year, month").show()

    spark.stop()


  }
```



## 2.Spark基于Python开发环境搭建

### 2.1 Hadoop环境搭建

```
同Scala
```

### 2.2 Python环境配置

```
需要通过minicanda  或者 uv 创建pyspark开发环境
pyspark 的版本是 3.1.2 匹配的python版本是 3.8 其他版本会报错
conda create --name bigdata_python3.8 python=3.8
pip install pyspark==3.1.2 -i https://pypi.tuna.tsinghua.edu.cn/simple
```

### 2.3 Pycharm创建pyspark项目

#### 2.3.1  pyspark 运行 sparkcore案例

案例数据准备

```
张山
hadoop
pathon
spark
flink
hive
hbase
clickhouse
elasticsearch
redis
scala
doris
presto
```



```python
from pyspark import SparkContext, SparkConf
import os

os.system('chcp 65001') # explicitly changed encoding to utf-8
os.environ['PYSPARK_PYTHON'] = "D:/Program Files/Miniconda3/envs/myplaywright-py3.8/python"
os.environ['PYSPARK_DRIVER_PYTHON'] = "D:/Program Files/Miniconda3/envs/myplaywright-py3.8/python"



if __name__ == '__main__':
    print("pyspark的入门案例: WordCount")

    # 1. 创建SparkContext核心对象:  spark Core 核心对象
    conf = SparkConf().setAppName('pyspark_wd').setMaster('local[*]')

    sc = SparkContext(conf=conf)
    # 2. 执行相关的操作:
    # 2.1 读取外部文件的数据
    # 路径写法: 协议 + 路径
    rdd_init = sc.textFile('file:///D:/learn/python/python-learn-demo/python-pyspark/data/wordcount.txt')
    # print(rdd_init.collect())

    # 2.2  对数据执行切割操作
    # flatMap : 一转多的时候
    rdd_flatMap = rdd_init.flatMap(lambda line: line.split(' '))
    # print(rdd_flatMap.collect())

    # 2.3 执行转换操作: 将每一个单词转换为 (单词,1)
    # map: 用于对列表中数据 进行一对一转换操作, 转换的方案取决于你传入的函数
    rdd_map = rdd_flatMap.map(lambda word: (word, 1))
    # print(rdd_map.collect())

    # 2.4  根据key进行分组聚合操作
    rdd_res = rdd_map.reduceByKey(lambda agg, curr: agg + curr)
    # 3. 打印结果
    print(rdd_res.collect())

    # 4- 释放资源
    sc.stop()
```





