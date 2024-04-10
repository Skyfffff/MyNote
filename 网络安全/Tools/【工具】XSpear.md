# XSpear

## 基本命令

```
使用方法: xspear -u [目标] -[选项] [值]
[ 例如 ]
$ xspear -u 'https://www.hahwul.com/?q=123' --cookie='role=admin' -v 1 -a 
$ xspear -u 'http://testphp.vulnweb.com/listproducts.php?cat=123' -v 2
$ xspear -u 'http://testphp.vulnweb.com/listproducts.php?cat=123' -v 0 -o json

[ 选项 ]
    -u, --url=target_URL             [必填] 目标 URL
    -d, --data=POST Body             [可选] POST 方法的请求体数据
    -a, --test-all-params            [可选] 测试所有参数（包括未反射的参数）
        --no-xss                     [可选] 不测试 XSS，仅分析参数
        --headers=HEADERS            [可选] 添加 HTTP 标头
        --cookie=COOKIE              [可选] 添加 Cookie
        --custom-payload=FILENAME    [可选] 加载自定义载荷的 JSON 文件
        --raw=FILENAME               [可选] 加载原始文件（例如 raw_sample.txt）
    -p, --param=PARAM                [可选] 测试参数
    -b, --BLIND=URL                  [可选] 添加盲 XSS 的向量
                                      + 与 XSS Hunter、ezXSS、HBXSS 等一起使用
                                      + 例如：-b https://hahwul.xss.ht
    -t, --threads=NUMBER             [可选] 线程数，默认为 10
    -o, --output=FORMAT              [可选] 输出格式 (cli , json)
    -c, --config=FILENAME            [可选] 使用 config.json
    -v, --verbose=0~3                [可选] 显示日志深度
                                      + v=0: 静默模式（仅显示结果）
                                      + v=1: 显示扫描状态（默认）
                                      + v=2: 显示扫描日志
                                      + v=3: 显示详细日志（请求/响应）
    -h, --help                       打印此帮助信息
        --version                    显示 XSpear 版本
        --update                     显示如何更新
```

## 特性

1、基于模式匹配的XSS扫描

2、检测无头浏览器的alert、confirm、prompt事件

3、针对XSS保护绕过来测试请求与响应

4、测试XSS盲注（XSS Hunter、ezXSS、HBXSS）

5、动态/静态分析：寻找SQL错误模式、分析安全Header、分析其他Header、测试URI路径

6、扫描元文件

7、基于Ruby开发（GEM库）

8、显示table base cli-report、filtered rule和testing raw query（url）

9、测试选中的参数

10、支持命令行JSON输出格式

11、支持Verbose 0-3级

12、支持Config文件

13、针对任意攻击向量支持自定义回调代码
