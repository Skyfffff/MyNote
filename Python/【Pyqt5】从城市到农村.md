# Pyqt5

## 槽函数线程池实现

Thread.py文件

```python
# 自定义信号类，也可以写在scanWorker里面
class WorkerSignals(QObject):
    update_data = pyqtSignal(list) # 自定义要穿出的数据类型
    finished = pyqtSignal(str)


class scanWorker(QRunnable):

    def __init__(self, *args, **kwargs):
        super(scanWorker, self).__init__(*args, **kwargs)

        self.signals = WorkerSignals()
        self.qq = None
        self.address = None

    # 用于外部传递参数
    def set_parameters(self, address, qq):
        self.address = address
        self.qq = qq
    
    # 线程执行的任务
    def run(self):
        print("xxx")
       	# 对主线程发出更新数据信号，并且传出数据
        self.signals.update_data.emit(card_list)
        # 发出结束标志
        self.signals.finished.emit(self.qq)
```

主文件

```python
class MainWindow(QMainWindow, Ui_MainWindow):
    def __init__(self):
        super(MainWindow, self).__init__()
        self.worker = None #也可以不创建事例变量
        self.setupUi(self)
        # 创建线程池
        self.threadpool = QThreadPool()

    def startScanning(self):
        # 其它代码

        # 创建多个线程并加入线程池
        for row in range(row_count):
            qq = self.tableWidget_Password.item(row, 1).text().strip()
            # 每次创建一个线程
            self.worker = Thread_Tasks.scanWorker()
            # 子线程信号绑定函数，用于传出信息或者结束标志
            self.worker.signals.update_data.connect(self.update_tableWidget)
            self.worker.signals.finished.connect(self.scan_finished)
            # 传入子线程需要的参数，也可以在构造器里面传入
            self.worker.set_parameters(address, qq)
            # 加入线程池开始执行
            self.threadpool.start(self.worker)

    def scan_finished(self, qq):
        print("扫描完成！")

    def update_tableWidget(self, card_list):
        print("更新数据操作...")
```