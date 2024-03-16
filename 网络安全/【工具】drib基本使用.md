# Dirb基本使用

```
dirb <url_base> [<wordlist_file(s)>] [options]

========================= 注意 =========================
 <url_base>：要扫描的基本 URL。（使用 -resume 以恢复会话）
 <wordlist_file(s)>：字典文件列表。（wordfile1,wordfile2,wordfile3...）

======================== 快捷键 ========================
 'n' -> 进入下一个目录。
 'q' -> 停止扫描。（保存状态以便恢复）
 'r' -> 显示剩余扫描统计信息。

======================== 选项 ========================
 -a <agent_string>：指定自定义 USER_AGENT。
 -b：使用原始路径。
 -c <cookie_string>：为 HTTP 请求设置 cookie。
 -E <certificate>：客户端证书的路径。
 -f：调整 NOT_FOUND（404）检测。
 -H <header_string>：向 HTTP 请求添加自定义标头。
 -i：使用不区分大小写的搜索。
 -l：在找到时打印 "Location" 标头。
 -N <nf_code>：忽略具有此 HTTP 代码的响应。
 -o <output_file>：将输出保存到磁盘。
 -p <proxy[:port]>：使用此代理。（默认端口为 1080）
 -P <proxy_username:proxy_password>：代理身份验证。
 -r：不进行递归搜索。
 -R：交互式递归。（针对每个目录询问）
 -S：静默模式。不显示已测试的单词。（适用于无法显示字符的终端）
 -t：不强制在 URL 上添加结尾的 '/'。
 -u <username:password>：HTTP 身份验证。
 -v：同时显示 NOT_FOUND 页面。
 -w：不在警告消息上停止。
 -X <extensions> / -x <exts_file>：为每个单词添加这些扩展名。
 -z <millisecs>：添加毫秒延迟，以避免过度洪水攻击。

======================== 示例 =======================
 dirb http://url/directory/（简单测试）
 dirb http://url/ -X .html（测试带有 '.html' 扩展名的文件）
 dirb http://url/ /usr/share/dirb/wordlists/vulns/apache.txt（使用 apache.txt 字典文件进行测试）
 dirb https://secure_url/（带 SSL 的简单测试）
```

