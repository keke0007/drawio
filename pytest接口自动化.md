# 接口自动化框架的开发

## 1.虚拟环境的准备

```
conda create --name pytest3.9 python=3.9 -i https://pypi.tuna.tsinghua.edu.cn/simple
```

## 2.Pycharm中配置terminal自动打开虚拟环境

```
我们需要在Pycharm>File>Tools>Terminal中切换powershell.exe为cmd.exe
```

## 3.requests模块知识点的学习

```
pip install requests -i https://pypi.tuna.tsinghua.edu.cn/simple
```



### 3.1Get请求案例

```
url_2 = 'http://127.0.0.1:8787/coupApply/cms/goodsList'
headers_2 = {'Content-Type': 'application/x-www-form-urlencoded;charset=utf-8'}
data_json = {
    "msgType": "getHandsetListOfCust",
    "page": 1,
    "size": 20
}
res_2 = requests.get(url=url_2, headers=headers_2, params=data_json)

res_2（Response 对象）常用属性/方法

res_2.status_code：整数状态码（200、404、500 等）。
res_2.text：把响应体解码为字符串（requests 根据响应头自动选择编码）。
res_2.content：原始字节（bytes），适合二进制文件。
res_2.json()：把响应体解析为 JSON（如果返回的是 JSON）。会在响应不是合法 JSON 时抛 ValueError。
res_2.headers：响应头字典。
res_2.url：最终请求的 URL（含查询字符串）。
res_2.request：原始发出的 PreparedRequest 对象，可以查看 res_2.request.method / .url / .headers / .body。
res_2.raise_for_status()：若状态码不是 2xx/3xx，会抛 HTTPError（便于异常控制流）。
res_2.elapsed：请求耗时（timedelta）
```

### 常见误区与改进建议（必须看）

1. **GET 带 Content-Type: application/x-www-form-urlencoded**
   - 这通常用于 POST 表单。GET 一般无需该 header；若你本意是发送表单到服务端并让服务读取 body，请改用 `POST` 并把数据放在 `data=`（form）或 `json=`（JSON body）中。
2. **未设置 timeout（可能导致请求挂起）**
   - 建议加 `timeout=秒数`，例如 `requests.get(..., timeout=5)`，避免请求无限等待。
3. **缺少异常处理**
   - 网络请求易失败（超时、连接错误、非 2xx 返回等），应捕获 `requests.exceptions` 并妥善处理／重试。
4. **没有重试/连接复用**
   - 大量短连接建议用 `requests.Session()` 复用连接；对不稳定的网络可配置 `HTTPAdapter` + `Retry`。
5. **JSON 解析失败**
   - 使用 `res_2.json()` 时要捕获 `ValueError` 或 `json.decoder.JSONDecodeError`，并检查 `Content-Type` 是否确认为 `application/json`。
6. **安全性**
   - 不要把敏感信息（密码、token）直接硬编码到代码中；用环境变量或安全存储。对外网请求注意 HTTPS 与证书验证（`verify=True` 默认启用）。

### 4️⃣ 总结对比表

| 参数     | HTTP 方法 | 放哪           | 编码           | 自动 Content-Type                              | 用法场景                 |
| -------- | --------- | -------------- | -------------- | ---------------------------------------------- | ------------------------ |
| `params` | GET/POST  | URL 查询字符串 | URL encode     | 无                                             | 查询参数，分页、搜索等   |
| `data`   | POST/PUT  | 请求体         | 表单编码或原始 | 仅表单默认 `application/x-www-form-urlencoded` | 发送表单数据，或二进制流 |
| `json`   | POST/PUT  | 请求体         | JSON encode    | 自动 `application/json`                        | 发送 JSON 数据给 API     |

------

### 5️⃣ 选择建议

- **GET 请求** → 用 `params`
- **POST 表单** → 用 `data`
- **POST/PUT JSON** → 用 `json`

### 3.2requests请求获取Cookie

```
url = 'http://127.0.0.1:8787/dar/user/login'
headers = {'Content-Type': 'application/x-www-form-urlencoded;charset=utf-8'}
data = {
    'user_name': 'test01',
    'passwd': 'admin123'
}

session = requests.session()
result = session.request('post', url=url, headers=headers, data=data)
cookie = requests.utils.dict_from_cookiejar(result.cookies)
print(cookie)
```

## 4.pyyaml模块知识点学习

### 4.1 读取yaml

```
login.yaml
- baseInfo:
    api_name: 用户登录
    url: /dar/user/login
    method: post
    header:
      Content-Type: application/x-www-form-urlencoded;charset=UTF-8
  testCase:
    - case_name: 用户名和密码正确登录验证
      data:
        user_name: test01
        passwd: admin123
      validation:
        - contains: { 'msg': '登录成功' }
      extract:
        token: $.token
        
    def read_yaml(self, file,default=None):
        if default is None:
            default = {}

        try:
            with open(file, 'r', encoding='utf-8') as f:
                data = yaml.safe_load(f)
                return data if data is not None else default
        except FileNotFoundError:
            print(f"YAML 文件不存在: {file}")
            return default
        except yaml.YAMLError as e:
            print(f"YAML 文件解析失败: {file}，错误信息: {e}")
            return default
        except Exception as e:
            print(f"读取 YAML 文件时发生未知错误: {file}，错误信息: {e}")
            return default
```

### 4.2 写入yaml

```
    def write_yaml(self,file_path,data):
        file_path = r'extract.yaml'
        if not os.path.exists(file_path):
            os.system(file_path)
        if not isinstance(data, (dict, list)):
            print("写入 YAML 的数据必须是 dict 或 list")
            return

        try:
            with open(file_path, 'a', encoding='utf-8') as f:
                yaml.dump(data, f, allow_unicode=True, sort_keys=False)
            print(f"成功写入 YAML 文件: {file_path}")
        except Exception as e:
            print(f"写入 YAML 文件失败: {file_path}, 错误: {e}")
```



### 4.3 json的序列化与反序列化

```
1.json的序列化
json序列化 其实就是将python的字典类型转换为字符串类型
data = {'key1': '春天', 'key2': '夏天'}
json_data = json.dumps(data,ensure_ascii=False,indent=4,sort_keys=True)print(json.dumps(data,ensure_ascii=False))

2.json的反序列化
json序列化 其实就是将python的字符串类型转换为字典类型
 print(type(json.loads(json_data)))
```

### 4.4yaml文件的动态解释

```python
python的{}中单引号的是字典类型dict,双引号的是字符串类型str

 def replace_loader(self, data):
        """
        yaml文件替换解析有${}格式的数据
        """
        if not isinstance(data, str):
            str_data = json.dumps(data,ensure_ascii=False)
            for i in range(str_data.count('${')):
                if "${" in str_data and "}" in str_data:
                    start_index = str_data.index('$')
                    end_index = str_data.index('}', start_index)
                    ref_all_params = str_data[start_index:end_index+1]

                    #取出函数名
                    func_name = ref_all_params[2:ref_all_params.index('(')]
                    print(func_name)
                    #取出函数里面的参数值
                    func_params= ref_all_params[ref_all_params.index('(')+1:ref_all_params.index(')')]
                    print(func_params)

                    #传入替换的参数获取对应的值
                    print("yaml文件替换解析前:",str_data)

                    extract_data=getattr(PlaceholderHelper(),func_name)(*func_params.split(',') if func_params else "")

                    str_data = str_data.replace(ref_all_params,str(extract_data))

                    print("yaml文件替换解析后:",str_data)

        #还原数据
        if data and isinstance(data, dict):
            data = json.loads(str_data)
        else:
            data = str_data

        return data
```

## 5.项目文件结构Python包

```
1.conf      配置文件目录 setting.py
2.base      基础目录
3.common    公共文件目录
4.data      存放测试数据的目录
5.report    存放测试报告的目录
6.testcase  存放测试用例的目录
7.log       存放log日志的目录
```

## 6.pytest框架

### 6.1 pytest的生态

```
1.pytest与requests结合实现接口自动化
2.pytest可以实现测试用力的跳过以及测试用例的失败重跑机制
3.pytest可以与allure结合生成美观的测试报告
4.pytest与Jenkins结合持续集成
5.pytest有非常丰富的插件功能
pytest-html---生成html格式的测试报告
pytest-xdist--测试用例分布式执行
pytest-ordering---用来改变我们测试用例的执行顺序
pytest-rerunfailures---测试用例失败后重跑
allure-pytest---用于生成美观的allure测试报告

插件的安装
pip install allure-pytest
```

### 6.2 pytest测试用例的使用规则以及基础使用

```
1.模块名称必须以test_开头或者以_test结尾
2.测试类必须以Test开头,并且不能有init方法
3.测试方法必须以test开头
```

### 6.3 pytest测试用例的运行方式

```
1.主函数运行方式  
if __name__ == '__main__':
    pytest.main()
 默认加载所有的模块
 
 指定模块运行
 if __name__ == '__main__':
    pytest.main(['-vs',./testcase/login/test_login.py'])
  pytest.main()带参数
  -s 表示输出调试信息,包括代码中的print打印信息
  -v 显示更加详细的信息
  -vs 两个参数结合起来使用
  -n  支持多进程或分布式运行测试用例
 pip install pytest-xdist -i https://pypi.tuna.tsinghua.edu.cn/simple/
 
 pytest -vs ./testcase -n 3
 指定运行某个文件夹下的所有测试用例
 if __name__ == '__main__':
    pytest.main(['-vs','./testcase'])
    
 pytest.main(['-vs','./testcase','-n 4'])

2.命令行运行 
D:\learn\python\pytest_demo>pytest

3.通过读取pytest.ini配置文件
1.pytest.ini是pytest框架的核心文件,文件名及后缀为固定写法,不可变更
2.文件一般放在项目的根目录
3.编码格式必须为ANSI格式,在pytest.ini文件不可以加上中文
4.作用是约定或者改变pytest默认的行为
5.运行规则是不管你是通过主函数运行还是通过命令行运行,他都会去读取pytest.ini配置文件
```

### 6.4 pytest的插件使用

```
1.-n 3 多线程执行测试用例
pip install pytest-xdist
pytest.main(['-vs','./testcase','-n 4'])
pytest -vs ./testcase -n 4

2.顺序执行测试用例
pip install pytest-ordering
@pytest.mark.run(order=3) 可以改变测试用例的执行顺序

3.测试失败之后用例重跑
pip install pytest-rerunfailures
pytest.main(['-vs','./testcase/login','--reruns=2'])
pytest -vs ./testcase/login --reruns=2 
失败的测试用例重跑次数为3次

4.分组冒烟测试
4.1 定义分组
markers =
    mapyan:冒烟测试
    usermanage:用户管理
4.2 在测试用例上面标记
@pytest.mark.mapyan
@pytest.mark.usermanage
4.3 执行分组用例
pytest ./testcase -m "maoyan"
pytest ./testcase -m "maoyan or usermanage"

5.测试用例的跳过
在测试用例上面标记
@pytest.mark.skip

6.测试用例生成报告
pip install allure-pytest

```

### 6.5 pytest测试框架前后置处理

#### 常见使用组合

```
1.setup_method/teardown_method setup_class/teardown_class
2.pytest核心,通过装饰器@pytest.fixtrue方式来实现用例前后置

3.conftest.py + @pytest.fixtrue
conftest.py文件名时固定写法,不可更改
这两个结合使用来实现全局前后置应用
使用场景: 1.登录操作 2.文件清除操作

```

#### setup/teardown

```
1.setup_method
def setup_method(self):
    print('在每个测试用例运前都执行我的代码块')
2.teardown_method
def teardown_method(self):
     print("在每个测试用例运行后都执行我的代码块")
3.setup_class
def setup_class(self):
    print('在所有测试用例前只执行一次,可以用来初始化对象,连接数据库')
4.teardown_class
def teardown_class(self):
    print("在所有测试用例执行之后只执行一次,可以用来关闭连接")
```

#### @pytest.fixtrue(scope="",params="",autouse="",ids="")

```
参数详解
scope
表示@pytest.fixtrue标记方法的作用域,他的值主要有4个
function(默认),class,module,package/session
function: 作用域是方法,就是每个测试用例执行之前都会先去执行前置操作,类似setup_method/teardown_down
class: 作用域是类,就是在每个类执行之前会执行的前置操作
module: 作用域是模块,当文件中有多个类时,只执行一次

params
表示参数化 (支持的格式有 list,tuple,dict)

autouse
自动使用,默认False
```

#### scope案例

```
@pytest.fixture(scope='function', autouse=True)
def fixtrue_test():
    print("测试用例执行的前置操作")
    yield
    print("测试用例执行的后置操作")
function表示 一个方法只执行一次
    
@pytest.fixture(scope='class', autouse=True)
def fixtrue_test():
    print("测试用例执行的前置操作")
    yield
    print("测试用例执行的后置操作")
class表示 一个类只执行一次 
 
 
@pytest.fixture(scope='module', autouse=True)
def fixtrue_test():
    print("测试用例执行的前置操作")
    yield
    print("测试用例执行的后置操作")
module表示 一个文件只执行一次    
    
@pytest.fixture(scope='session', autouse=True)
def fixtrue_test():
    print("测试用例执行的前置操作")
    yield
    print("测试用例执行的后置操作")
session作用表示多个文件只执行一次
```

#### params案例

```
@pytest.fixture(scope='function', autouse=True,params=['北京','上海','深圳'])
def fixtrue_test(request):
    return request.param
    
def test_login3(self,fixtrue_test):
     print(f'用例3的params{fixtrue_test}')
```

ids

```
当使用params参数化时,给每一个值设置一个变量,意义不大
```

name

```
给fixtrue_test方法取别名,调用fixtrue_test可以直接使用别用
```

#### pytest的 参数化

```
@pytest.mark.parametrize('params',['张山','李四','王五'])

args_name 参数名称
args_value 值
值的类型有  list,tuple,[dict],(dict)

very important
参数里面有多少个值 这个用例就会执行多少次

@pytest.mark.parametrize('params',['张山','李四','王五'])
    def test_login1(self,params):
        print('用例1')
        print(f'获取到的参数值---{params}')
```

#### pytest使用yaml做数据驱动

```
@pytest.mark.parametrize('params', get_testcase_yaml('./testcase/login/login.yaml'))
    def test_login2(self,params):
        print('用例2')
        print(params)
```

6.6 配置allure展示测试用例

```
1️⃣ 下载 Allure

打开 Allure 官方 Release 页面：
👉 https://github.com/allure-framework/allure2/releases
2️⃣ 解压
2️⃣ 配置环境变量
3️⃣ 配置环境变量
4️⃣ 验证安装
allure --version
5️⃣ 使用 Allure 查看报告
allure serve ./report/temp

```

### 7.封装日志

```python
log_path = setting.FILE_PATH['log']

if not os.path.exists(log_path):
    os.makedirs(log_path)
LOG_FILE_NAME = log_path + r'\test.{}.log'.format(str(time.strftime('%Y%m%d')))

class RecordLog:
    """
    封装日志
    """
    def output_logging(self):
        """ 获取logger对象"""
        logger = logging.getLogger(__name__)
        # 防止打印重复
        if not logger.handlers:
            logger.setLevel(setting.LOG_LEVEL)
            log_formatter = logging.Formatter(
                '%(levelname)s - %(asctime)s - %(filename)s:%(lineno)d -[%(module)s:%(funcName)s] - %(message)s')
            # 日志输出到指定文件
            fh = RotatingFileHandler(filename=LOG_FILE_NAME, mode='a', maxBytes=5242880,
                                     backupCount=7,
                                     encoding='utf-8')
            fh.setLevel(setting.LOG_LEVEL)
            fh.setFormatter(log_formatter)

            # 在将相应的handler添加到logger
            logger.addHandler(fh)

            # 将日志添加到控制台上
            sh = logging.StreamHandler()
            sh.setLevel(setting.STREAM_LOG_LEVEL)
            sh.setFormatter(log_formatter)

            logger.addHandler(sh)
        return logger



apilog = RecordLog()
logs = apilog.output_logging()
```

