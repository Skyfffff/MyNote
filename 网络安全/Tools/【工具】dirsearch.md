# dirsearch基本用法

## 简单使用

```
dirsearch -u https://target
dirsearch -e php,html,js -u https://target
dirsearch -e php,html,js -u https://target -w /path/to/wordlist
```

## 常用命令

扫描状态码200的php,Html,js页面并且输出到文件中

```
dirsearch -u http://192.168.229.128 -e php,html,js -i 200 -o output.txt --format=simple
```

## 中文翻译

```
用法: dirsearch.py [-u|--url] 目标 [-e|--extensions] 扩展名 [选项]

选项:
  --version             显示程序版本号并退出
  -h, --help            显示此帮助消息并退出

  必选项:
    -u URL, --url=URL   目标 URL(s)，可以使用多个标志
    -l PATH, --urls-file=PATH
                        URL 列表文件
    --stdin             从标准输入读取 URL(s)
    --cidr=CIDR         目标 CIDR
    --raw=PATH          从文件加载原始 HTTP 请求（使用 '--scheme' 标志设置协议）
    -s SESSION_FILE, --session=SESSION_FILE
                        会话文件
    --config=PATH       配置文件路径（默认为 'DIRSEARCH_CONFIG' 环境变量，否则为 'config.ini'）

  字典设置:
    -w WORDLISTS, --wordlists=WORDLISTS
                        字典文件或包含字典文件的目录（用逗号分隔）
    -e EXTENSIONS, --extensions=EXTENSIONS
                        逗号分隔的扩展名列表（例如 php,asp）
    -f, --force-extensions
                        将扩展名添加到每个字典条目的末尾。默认情况下，dirsearch 仅用扩展名替换 %EXT% 关键字
    -O, --overwrite-extensions
                        用您选择的扩展名（通过 `-e` 选择）覆盖字典中的其他扩展名
    --exclude-extensions=EXTENSIONS
                        排除的扩展名列表（以逗号分隔，例如 asp,jsp）
    --remove-extensions
                        从所有路径中删除扩展名（例如 admin.php -> admin）
    --prefixes=PREFIXES
                        将自定义前缀添加到所有字典条目（用逗号分隔）
    --suffixes=SUFFIXES
                        将自定义后缀添加到所有字典条目，忽略目录（用逗号分隔）
    -U, --uppercase     字典转换为大写
    -L, --lowercase     字典转换为小写
    -C, --capital       字典首字母大写

  通用设置:
    -t THREADS, --threads=THREADS
                        线程数
    -r, --recursive     递归暴力破解
    --deep-recursive    在每个目录深度执行递归扫描（例如 api/users -> api/）
    --force-recursive   对每个找到的路径执行递归暴力破解，而不仅仅是目录
    -R DEPTH, --max-recursion-depth=DEPTH
                        最大递归深度
    --recursion-status=CODES
                        执行递归扫描的有效状态码，支持范围（以逗号分隔）
    --subdirs=SUBDIRS   扫描给定 URL[s] 的子目录（以逗号分隔）
    --exclude-subdirs=SUBDIRS
                        在递归扫描期间排除以下子目录（以逗号分隔）
    -i CODES, --include-status=CODES
                        包括状态码，以逗号分隔，支持范围（例如 200,300-399）
    -x CODES, --exclude-status=CODES
                        排除状态码，以逗号分隔，支持范围（例如 301,500-599）
    --exclude-sizes=SIZES
                        根据大小排除响应，以逗号分隔（例如 0B,4KB）
    --exclude-text=TEXTS
                        根据文本排除响应，可以使用多个标志
    --exclude-regex=REGEX
                        根据正则表达式排除响应
    --exclude-redirect=STRING
                        如果匹配重定向 URL，则排除响应（例如 '/index.html'）
    --exclude-response=PATH
                        排除类似于此页面响应的响应，路径作为输入（例如 404.html）
    --skip-on-status=CODES
                        每当遇到这些状态码之一时跳过目标，以逗号分隔，支持范围
    --min-response-size=LENGTH
                        最小响应长度
    --max-response-size=LENGTH
                        最大响应长度
    --max-time=SECONDS  扫描的最长运行时间
    --exit-on-error     每当出现错误时退出

  请求设置:
    -m METHOD, --http-method=METHOD
                        HTTP 方法（默认为 GET）
    -d DATA, --data=DATA
                        HTTP 请求数据
    --data-file=PATH    包含 HTTP 请求数据的文件
    -H HEADERS, --header=HEADERS
                        HTTP 请求头，可以使用多个标志
    --headers-file=PATH
                        包含 HTTP 请求头的文件
    -F, --follow-redirects
                        跟随 HTTP 重定向
    --random-agent      为每个请求选择随机 User-Agent
    --auth=CREDENTIAL   认证凭据（例如 user:password 或 bearer token）
    --auth-type=TYPE    认证类型（basic、digest、bearer、ntlm、jwt）
    --cert-file=PATH    包含客户端证书的文件
    --key-file=PATH     包含客户端证书私钥（未加密）的文件
    --user-agent=USER_AGENT
    --cookie=COOKIE

  连接设置:
    --timeout=TIMEOUT   连接超时
    --delay=DELAY       请求之间的延迟
    -p PROXY, --proxy=PROXY
                        代理 URL（HTTP/SOCKS），可以使用多个标志
    --proxies-file=PATH
                        包含代理服务器的文件
    --proxy-auth=CREDENTIAL
                        代理认证凭据
    --replay-proxy=PROXY
                        用找到的路径重放的代理
    --tor               使用 Tor 网络作为代理
    --scheme=SCHEME     原始请求的协议或 URL 中没有协议时的协议（默认为自动检测）
    --max-rate=RATE     每秒最大请求数
    --retries=RETRIES   失败请求的重试次数
    --ip=IP             服务器 IP 地址
    --interface=NETWORK_INTERFACE
                        要使用的网络接口

  高级设置:
    --crawl             在响应中进行新路径的爬取

  查看设置:
    --full-url          输出中包含完整的 URL（在安静模式下自动启用）
    --redirects-history
                        显示重定向历史记录
    --no-color          无彩色输出
    -q, --quiet-mode    安静模式

  输出设置:
    -o PATH/URL, --output=PATH/URL
                        输出文件或 MySQL/PostgreSQL URL（格式为 scheme://[username:password@]host[:port]/database-name）
    --format=FORMAT     报告格式（可用：simple,plain,json,xml,md,csv,html,sqlite,mysql,postgresql）
    --log=PATH          日志文件

请参阅 'config.ini' 以获取示例配置文件

```

