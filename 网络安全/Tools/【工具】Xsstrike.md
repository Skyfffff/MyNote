# XSStrike

## 常用命令

### 扫描Get请求

```shell
python3 xsstrike.py -u "http://192.168.40.129/xsslabs/level2.php"
```

### 扫描Post请求

```shell
python3 xsstrike.py -u "http://192.168.40.129/xsslabs/level1.php" --data "message=1&submit=submit" --headers "Cookie: ant[uname]=admin;"
```

## 官方使用说明

[Usage · s0md3v/XSStrike Wiki (github.com)](https://github.com/s0md3v/XSStrike/wiki/Usage)

使用 xsstrike.py 进行跨站脚本（XSS）漏洞扫描的命令行选项如下：

可选参数：
- `-h` 或 `--help`：显示帮助信息并退出程序。
- `-u` 或 `--url`：指定目标 URL。
- `--data`：提供 POST 数据。
- `-f` 或 `--file`：从文件加载攻击载荷。
- `-t` 或 `--threads`：设置线程数。
- `-l` 或 `--level`：指定爬取深度。
- `-t` 或 `--encode`：设置载荷编码方式。
- `--json`：将 POST 数据视为 JSON 格式处理。
- `--path`：在 URL 路径中注入载荷。
- `--seeds`：从文件加载 URL 作为爬取种子。
- `--fuzzer`：启动模糊测试功能。
- `--update`：更新工具。
- `--timeout`：设置请求超时时间。
- `--params`：查找参数。
- `--crawl`：启动爬虫模式进行目标发现与测试。
- `--proxy`：使用代理（或代理列表）。
- `--blind`：在爬取过程中注入盲注 XSS 载荷。
- `--skip-dom`：跳过 DOM 检查。
- `--headers`：添加 HTTP 头部。
- `-d` 或 `--delay`：设置请求之间的延迟。

**扫描单一 URL**
选项：`-u` 或 `--url`
针对使用 GET 方法的单个网页进行测试。
示例：

```bash
python xsstrike.py -u "http://example.com/search.php?q=query"
```

**提供 POST 数据**
示例：

```bash
python xsstrike.py -u "http://example.com/search.php" --data "q=query"
```

**测试 URL 路径组件**
选项：`--path`
如果希望在类似 `http://example.com/search/<payload>` 的 URL 路径中注入载荷，可以使用 `--path` 选项。
示例：

```bash
python xsstrike.py -u "http://example.com/search/form/query" --path
```

**将 POST 数据视为 JSON**
选项：`--json`
此选项用于通过 POST 方法测试 JSON 数据。
示例：

```bash
python xsstrike.py -u "http://example.com/search.php" --data '{"q":"query"}' --json
```

**爬取**
选项：`--crawl`
从目标网页开始爬取并测试发现的链接。
示例：

```bash
python xsstrike.py -u "http://example.com/page.php" --crawl
```

**爬取深度**
选项：`-l` 或 `--level` | 默认值：2
此选项用于指定爬取的深度级别。
示例：

```bash
python xsstrike.py -u "http://example.com/page.php" --crawl -l 3
```

**从文件测试/爬取 URL**
选项：`--seeds`
若要测试来自文件的 URL 或仅为爬取添加种子 URL，可使用 `--seeds` 选项。
示例：

```bash
python xsstrike.py --seeds urls.txt
```
或
```bash
python xsstrike.py -u "http://example.com" -l 3 --seeds urls.txt
```

**暴力破解载荷文件**
选项：`-f` 或 `--file`
可以从文件加载载荷并检查其有效性。在此模式下，XSStrike 不会进行任何分析。
示例：

```bash
python3 xsstrike.py -u "http://example.com/page.php?q=query" -f /path/to/file.txt
```

**盲注 XSS**
选项：`--blind`
在爬取时使用此选项，XSStrike 将向每个 HTML 表单的所有参数注入在 `core/config.py` 中定义的盲注 XSS 载荷。
示例：

```bash
python xsstrike.py -u http://example.com/page.php?q=query --crawl --blind
```

**载荷编码**
选项：`-e` 或 `--encode`
XSStrike 支持按需对载荷进行编码。目前支持的编码方式包括：

- base64
示例：
```bash
python xsstrike.py -u "http://example.com/page.php?q=query" -e base64
```

**模糊测试**
选项：`--fuzzer`
模糊测试器旨在测试过滤器和 Web 应用防火墙。由于它发送随机延迟请求，且延迟可能长达 30 秒，因此速度非常慢。为了减少延迟，可以通过 `-d` 选项将其设置为 1 秒。
示例：

```bash
python xsstrike.py -u "http://example.com/search.php?q=query" --fuzzer
```

**日志记录**
选项：`--console-log-level` | 默认值：INFO
可以选择一个最低日志级别，以便在控制台显示 xsstrike 的日志：

```bash
python xsstrike.py -u "http://example.com/search.php?q=query" --console-log-level WARNING
```

选项：`--file-log-level` | 默认值：None
如果指定了该选项，xsstrike 还会将等于或高于所指定日志级别的所有日志写入文件：

```bash
python xsstrike.py -u "http://example.com/search.php?q=query" --console-log-level DEBUG
```

选项：`--log-file` | 默认值：xsstrike.log
指定日志文件的名称。注意，如果未指定 `--file-log-level`，此选项将不会产生任何效果。
```bash
python xsstrike.py -u "http://example.com/search.php?q=query" --file-log-level INFO --log-file output.log
```

**使用代理**
选项：`--proxy` | 默认值：0.0.0.0:8080
需要在 `core/config.py` 中配置代理（或代理列表），然后通过 `--proxy` 开关在需要时使用它们。
有关设置代理的更多详细信息，请参见此处。

**跳过确认提示**
选项：`--skip`
如果希望在检测到有效载荷后，让 XSStrike 继续扫描而不询问是否继续扫描，可以使用此选项。它也会跳过 POC 生成。
示例：
```bash
python xsstrike.py -u "http://example.com/search.php?q=query" --skip
```

**跳过 DOM 扫描**
选项：`--skip-dom`
在爬取时可能希望跳过 DOM XSS 扫描以节省时间。
示例：
```bash
python xsstrike.py -u "http://example.com/search.php?q=query" --skip-dom
```

**更新工具**
选项：`--update`

**提供 HTTP 头部**
选项：`--headers`
此选项会打开文本编辑器（默认为 'nano'），您可以在其中粘贴 HTTP 头部并按 Ctrl + S 保存。
如果您的操作系统不支持这种方式，或者您不想这样做，也可以直接从命令行以 `\n` 分隔添加头部：

```bash
python xsstrike.py -u http://example.com/page.php?q=query --headers "Accept-Language: en-US\nCookie: null"
```