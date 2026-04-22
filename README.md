# PythonTestProject 接口自动化测试项目
## 项目简介
本项目基于 **Python + Pytest + Requests + Allure** 搭建的轻量级、易扩展**接口自动化测试框架**，
集成配置管理、日志收集、数据解析、自定义断言、邮件/钉钉通知、可视化测试报告、多格式数据驱动能力，
适用于后端 HTTP 接口自动化回归测试、冒烟测试、日常迭代接口验收。

## 技术栈
- 运行环境：Python 3.8+
- 测试框架：`pytest`
- 网络请求：`requests`
- 可视化报告：`allure-pytest`
- 配置/数据解析：`pyyaml、configparser、openpyxl`
- 扩展能力：钉钉机器人、邮件推送、Jenkins 集成、日志持久化

## 项目目录结构
```
PythonTestProject/
├── conf/                # 全局配置目录
│   ├── config.ini       # 环境、数据库、机器人等配置项
│   ├── operationConfig.py # 配置文件读取封装
│   └── setting.py       # 全局路径、常量配置
├── common/              # 公共工具类库
│   ├── sendrequest.py   # 统一HTTP请求封装（GET/POST/PUT/DELETE）
│   ├── assertions.py    # 自定义断言封装
│   ├── readyaml.py      # YAML文件读写工具
│   ├── handleExcel.py   # Excel测试数据读取
│   ├── recordlog.py     # 全局日志封装
│   ├── dingRobot.py     # 钉钉消息推送
│   ├── semail.py        # 邮件报告推送
│   └── 其他通用工具
├── testcase/            # 测试用例目录（按业务模块拆分）
├── data/                # 测试数据目录（Excel/CSV/YAML）
├── logs/                # 运行日志输出目录
├── report/              # 测试报告目录
│   └── allureReport/    # Allure可视化报告
├── base/                # 基础封装层、公共业务基类
├── extract.yaml         # 接口关联参数提取配置
├── pytest.ini           # pytest全局配置
├── run.py               # 项目统一启动入口
├── requirements.txt     # 第三方依赖清单
└── 使用前请阅读此文件.md
```

## 环境部署
### 1. 克隆项目
```bash
git clone https://github.com/Xkzx520/PythonTestProject.git
cd PythonTestProject
```

### 2. 安装依赖
```bash
pip install -r requirements.txt
```

### 3. 前置准备
1. 本地安装 **Allure** 命令行工具（用于生成可视化报告）
2. 修改 `conf/config.ini` 配置：
   - 替换测试环境域名
   - 按需配置数据库、钉钉机器人、邮件信息

## 快速使用
### 1. 编写测试用例
1. 在 `testcase/` 目录下新建测试文件，命名规范：`test_模块名.py`
2. 引入项目封装的请求、断言、配置工具类
3. 采用 pytest 语法编写用例，支持参数化、夹具、跳过/失败重跑

### 2. 执行测试
#### 方式一：一键运行（推荐）
```bash
python run.py
```
自动执行全部用例、生成 Allure 报告、输出日志、支持消息推送。

#### 方式二：pytest 命令灵活执行
```bash
# 执行全部用例
pytest testcase/ -v -s

# 执行指定单文件
pytest testcase/test_login.py -v

# 生成allure临时报告
pytest testcase/ --alluredir=report/allureReport/temp
allure serve report/allureReport/temp
```

## 核心功能说明
1. **统一请求封装**
   封装常用 HTTP 请求方法，统一处理请求头、超时、异常捕获，减少重复代码。

2. **自定义断言**
   支持状态码校验、文本包含、字段相等、key 存在性、数据范围等通用断言。

3. **接口关联**
   通过 `extract.yaml` 实现 token、id 等接口依赖参数提取与全局复用。

4. **多数据驱动**
   支持 Excel / YAML / CSV 外部文件管理测试数据，实现用例与数据分离。

5. **日志全覆盖**
   全程记录请求参数、响应结果、错误堆栈，按日期分割日志，方便问题定位。

6. **结果通知**
   测试完成自动推送测试结果至**钉钉群 / 邮箱**，适配 CI/CD 持续集成。

7. **可视化报告**
   Allure 精美报告展示：用例通过率、失败原因、请求详情、执行耗时。

## 配置说明
- `conf/config.ini`：环境地址、数据库、机器人密钥、超时时间
- `conf/setting.py`：项目根目录、日志路径、报告路径全局统一管理
- `pytest.ini`：pytest 运行规则、用例命名规则、标记配置

## 持续集成（可选）
可结合 Jenkins 实现：
- 定时自动化执行接口回归
- 构建自动触发测试
- 测试报告归档、失败结果告警

## 常见问题
1. 依赖安装报错
> 升级 pip 后重试：`pip install --upgrade pip`

2. Allure 报告无法打开
> 检查本地是否配置 Allure 环境变量，确认命令行可执行 `allure --version`

3. 接口请求失败
> 核对 `config.ini` 环境地址、网络代理、接口鉴权信息

## 作者&维护
- 仓库地址：https://github.com/Xkzx520/PythonTestProject.git
- 用途：个人学习、接口自动化测试练习、项目快速脚手架
