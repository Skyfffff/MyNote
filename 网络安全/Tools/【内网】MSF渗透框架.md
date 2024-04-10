# MSF

## msfconsole命令

### 核心命令

    命令          描述
    -------       -----------
    ?             帮助菜单
    banner        显示一个酷炫的Metasploit标语
    cd            更改当前工作目录
    color         切换颜色
    connect       与主机通信
    debug         显示用于调试的有用信息
    exit          退出控制台
    features      显示可以选择加入的尚未发布功能的列表
    get           获取上下文特定变量的值
    getg          获取全局变量的值
    grep          对另一个命令的输出进行grep
    help          帮助菜单
    history       显示命令历史
    load          加载框架插件
    quit          退出控制台
    repeat        重复一系列命令
    route         通过会话路由流量
    save          保存活动数据存储
    sessions      转储会话列表并显示有关会话的信息
    set           将上下文特定变量设置为值
    setg          将全局变量设置为值
    sleep         休眠指定的秒数
    spool         将控制台输出写入文件和屏幕
    threads       查看和操作后台线程
    tips          显示有用的生产力提示
    unload        卸载框架插件
    unset         取消设置一个或多个上下文特定变量
    unsetg        取消设置一个或多个全局变量
    version       显示框架和控制台库的版本号

### 模块命令

    命令          描述
    -------       -----------
    advanced      显示一个或多个模块的高级选项
    back          从当前上下文返回
    clearm        清除模块堆栈
    favorite      将模块添加到喜爱的模块列表
    favorites     打印喜爱的模块列表（别名为“show favorites”）
    info          显示一个或多个模块的信息
    listm         列出模块堆栈
    loadpath      从路径搜索并加载模块
    options       显示全局选项或一个或多个模块的选项
    popm          弹出堆栈中的最新模块并使其处于活动状态
    previous      将先前加载的模块设置为当前模块
    pushm         推送活动模块或模块列表到模块堆栈
    reload_all    从所有定义的模块路径重新加载所有模块
    search        搜索模块名称和描述
    show          显示给定类型的模块或所有模块
    use           通过名称或搜索词/索引与模块互动

### 任务命令

    命令          描述
    -------       -----------
    handler       启动一个作业作为有效载荷处理程序
    jobs          显示和管理作业
    kill          终止作业
    rename_job    重命名作业

### 资源脚本命令

    命令          描述
    -------       -----------
    makerc        将自启动以来输入的命令保存到文件中
    resource      运行存储在文件中的命令

### 数据库后端命令

    命令          描述
    -------       -----------
    analyze       分析关于特定地址或地址范围的数据库信息
    db_connect    连接到现有数据服务
    db_disconnect 断开与当前数据服务的连接
    db_export     导出包含数据库内容的文件
    db_import     导入扫描结果文件（文件类型将自动检测）
    db_nmap       执行nmap并自动记录输出
    db_rebuild_cache  重建数据库存储的模块缓存（不建议使用）
    db_remove     删除保存的数据服务条目
    db_save       将当前数据服务连接保存为启动时重新连接的默认值
    db_status     显示当前数据服务状态
    hosts         列出数据库中的所有主机
    klist         列出数据库中的Kerberos票证
    loot          列出数据库中的所有战利品
    notes         列出数据库中的所有注释
    services      列出数据库中的所有服务
    vulns         列出数据库中的所有漏洞
    workspace     在数据库工作区之间切换

### 凭证后端命令

    命令          描述
    -------       -----------
    creds         列出数据库中的所有凭证

### 开发者命令

    命令          描述
    -------       -----------
    edit          编辑当前模块或首选编辑器中的文件
    irb           在当前上下文中打开交互式Ruby shell
    log           显示framework.log，如果可能的话，到底部
    pry           在当前模块或Framework上打开Pry调试器
    reload_lib    从指定路径重新加载Ruby库文件
    time          计算运行特定命令所需的时间

### msfconsole

`msfconsole`是Metasploit Framework的主要界面。还有很多需要在这里进行的操作，请耐心等待并关注这个空间！

### 构建范围和列表

许多需要使用范围来避免手动列出每个所需的项的命令和选项。所有范围都是包容性的。

#### ID范围

接受ID列表的命令可以使用范围来帮助。单独的ID必须用`,`（不允许空格）分开，范围可以用`-`或`..`表示。

#### IP范围

有几种指定IP地址

范围的方式可以混合在一起。第一种方式是以一个` `（ASCII空格）分隔的IP列表，可选加逗号。下一种方式是以`BEGINNING_ADDRESS-END_ADDRESS`的形式的两个完整IP地址，比如`127.0.1.44-127.0.2.33`。也可以使用CIDR规范，但必须将整个地址给Metasploit，例如`127.0.0.0/8`而不是`127/8`，与RFC相反。此外，可以结合使用网掩码和域名来动态解析要定位的块。所有这些方法都适用于IPv4和IPv6地址。IPv4地址也可以使用来自[NMAP目标规范](https://nmap.org/book/man-target-specification.html)的特殊八进制范围指定。

#### 示例

终止第一个会话：

    sessions -k 1

停止一些额外运行的作业：

    jobs -k 2-6,7,8,11..15

检查一组IP地址：

    check 127.168.0.0/16, 127.0.0-2.1-4,15 127.0.0.255

定位一组IPv6主机：

    set RHOSTS fe80::3990:0000/110, ::1-::f0f0

以解析的域名为目标的块：

    set RHOSTS www.example.test/24

## meterpreter命令

以下是Meterpreter中的核心命令以及标准API的子命令，包括文件系统、网络、系统、用户界面、摄像头和音频输出命令：

### 核心命令

- `?`: 帮助菜单
- `background` / `bg`: 将当前会话放到后台
- `bgkill`: 终止后台运行的Meterpreter脚本
- `bglist`: 列出正在后台运行的脚本
- `bgrun`: 以后台线程的形式执行Meterpreter脚本
- `channel`: 显示活动通道的信息或控制它们
- `close`: 关闭一个通道
- `detach`: 从HTTP/HTTPS会话中分离Meterpreter会话
- `disable_unicode_encoding`: 禁用Unicode字符串编码
- `enable_unicode_encoding`: 启用Unicode字符串编码
- `exit`: 终止Meterpreter会话
- `get_timeouts`: 获取当前会话超时值
- `guid`: 获取会话的GUID
- `help`: 帮助菜单
- `info`: 显示有关Post模块的信息
- `irb`: 在当前会话上打开交互式Ruby shell
- `load`: 加载一个或多个Meterpreter扩展
- `machine_id`: 获取与会话关联的机器的MSF ID
- `migrate`: 将服务器迁移到另一个进程
- `pivot`: 管理轴线监听器
- `pry`: 在当前会话上打开Pry调试器
- `quit`: 终止Meterpreter会话
- `read`: 从通道中读取数据
- `resource`: 运行存储在文件中的命令
- `run`: 执行Meterpreter脚本或Post模块
- `secure`: （重新）协商会话上的TLV数据包加密
- `sessions`: 快速切换到另一个会话
- `set_timeouts`: 设置当前会话的超时值
- `sleep`: 强制Meterpreter保持静默，然后重新建立会话
- `ssl_verify`: 修改SSL证书验证设置
- `transport`: 管理传输机制
- `use`: 已弃用的别名，现在用于"load"
- `uuid`: 获取当前会话的UUID
- `write`: 将数据写入通道

### 文件系统命令

- `cat`: 读取文件内容并显示在屏幕上
- `cd`: 切换目录
- `checksum`: 获取文件的校验和
- `cp`: 复制源文件到目标位置
- `del`: 删除指定文件
- `dir` / `ls`: 列出文件（`ls`是`dir`的别名）
- `download`: 下载文件或目录
- `edit`: 编辑文件
- `getlwd`: 打印本地工作目录
- `getwd`: 打印工作目录
- `lcat`: 读取本地文件内容并显示在屏幕上
- `lcd`: 切换本地工作目录
- `lls`: 列出本地文件
- `lpwd`: 打印本地工作目录
- `ls`: 列出文件
- `mkdir`: 创建目录
- `mv`: 移动源文件到目标位置
- `pwd`: 打印工作目录
- `rm`: 删除指定文件
- `rmdir`: 删除目录
- `search`: 搜索文件
- `show_mount`: 列出所有挂载点/逻辑驱动器
- `upload`: 上传文件或目录

### 网络命令

- `arp`: 显示主机ARP缓存
- `getproxy`: 显示当前代理配置
- `ifconfig`: 显示接口信息
- `ipconfig`: 显示接口信息
- `netstat`: 显示网络连接
- `portfwd`: 将本地端口转发到远程服务
- `resolve`: 解析目标上的一组主机名
- `route`: 查看并修改路由表

### 系统命令

- `clearev`: 清除事件日志
- `drop_token`: 放弃任何活动的模拟令牌
- `execute`: 执行命令
- `getenv`: 获取一个或多个环境变量的值
- `getpid`: 获取当前进程标识符
- `getprivs`: 尝试启用当前进程可用的所有特权
- `getsid`: 获取服务器正在运行的用户的SID
- `getuid`: 获取服务器正在运行的用户
- `kill`: 终止进程
- `localtime`: 显示目标系统的本地日期和时间
- `pgrep`: 按名称过滤进程
- `pkill`: 按名称终止进程
- `ps`: 列出运行中的进程
- `reboot`: 重新启动远程计算机
- `reg`: 修改和与远程注册表交互
- `rev2self`: 在远程机器上调用RevertToSelf()
- `shell`: 进入系统命令shell
- `shutdown`: 关闭远程计算机
- `steal_token`: 尝试从目标进程中窃取模拟令牌
- `suspend`: 暂停或恢复一组进程
- `sysinfo`: 获取有关远程系统的信息，如操作系统信息

### 用户界面命令

- `enumdesktops`: 列出所有可访问的桌面和窗口工作站
- `getdesktop`: 获取当前Meterpreter桌面
- `idletime`: 返回远程用户的闲置秒数
- `keyboard_send`: 发送按键
- `keyevent`: 发送键盘事件
- `keyscan_dump`: 转储按键缓冲区
- `keyscan_start`: 开始捕获按键
- `keyscan_stop`: 停止捕获按键
- `mouse`: 发送鼠标事件

- `screenshare`: 观看远程用户桌面的实时画面
- `screenshot`: 获取交互式桌面的截图
- `setdesktop`: 更改Meterpreter的当前桌面
- `uictl`: 控制一些用户界面组件

### 摄像头命令

- `record_mic`: 从默认麦克风录制音频（X秒）
- `webcam_chat`: 开始视频聊天
- `webcam_list`: 列出摄像头
- `webcam_snap`: 从指定摄像头获取快照
- `webcam_stream`: 播放指定摄像头的视频流

### 音频输出命令

- `play`: 在目标系统上播放波形音频文件（.wav）

### Priv: 提权命令

- `getsystem`: 尝试提权至本地系统权限。

### Priv: 密码数据库命令

- `hashdump`: 转储SAM数据库的内容

### Priv: 时间戳命令

- `timestomp`: 操纵文件的MACE属性

## KIWI

原名mimikatz

    命令         描述
    -------       -----------
    creds_all     检索所有凭据（解析过的）
    creds_kerber  检索Kerberos凭据（解析过的）
    os
    creds_livess  检索Live SSP凭据
    p
    creds_msv     检索LM/NTLM凭据（解析过的）
    creds_ssp     检索SSP凭据
    creds_tspkg   检索TsPkg凭据（解析过的）
    creds_wdiges  检索WDigest凭据（解析过的）
    t
    dcsync        通过DCSync检索用户账户信息（未解析的）
    dcsync_ntlm   通过DCSync检索用户账户NTLM哈希、SID和RID
    golden_ticke  创建一个黄金Kerberos票据
    t_create
    kerberos_tic  列出所有Kerberos票据（未解析的）
    ket_list
    kerberos_tic  清除任何正在使用的Kerberos票据
    ket_purge
    kerberos_tic  使用Kerberos票据
    ket_use
    kiwi_cmd      执行任意mimikatz命令（未解析的）
    lsa_dump_sam  转储LSA SAM（未解析的）
    lsa_dump_sec  转储LSA secrets（未解析的）
    rets
    password_cha  更改用户的密码/哈希
    nge
    wifi_list     列出当前用户的WiFi配置文件/凭据
    wifi_list_sh  列出共享的WiFi配置文件/凭据（需要SYSTEM权限）