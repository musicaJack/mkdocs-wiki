``` py title="async_event_system.py" linenums="1"  hl_lines="120-126"
#**本代码片段仅用于测试MARKDOWN文档格式**
import uasyncio as asyncio
from machine import Pin

# ==============================================
# 异步队列实现（支持跨协程安全通信）
# ==============================================
class SimpleAsyncQueue:
    """
    简化版异步队列实现
    特性：
    - 线程不安全但协程安全
    - 适用于单生产者单消费者场景
    - 依赖asyncio.Event实现高效等待
    """
    def __init__(self):
        self._queue = []          # 实际存储元素的列表
        self._evt = asyncio.Event()  # 用于协程间通知的事件对象
    
    async def put(self, item):
        """ 生产者方法：向队列添加元素 """
        self._queue.append(item)  # 添加元素到队列尾部
        self._evt.set()           # 触发事件通知消费者
    
    async def get(self):
        """ 消费者方法：从队列获取元素（队列空时阻塞等待） """
        while not self._queue:
            await self._evt.wait()  # 等待生产者通知
            self._evt.clear()       # 重置事件状态
        return self._queue.pop(0)   # 从队列头部取出元素

# ==============================================
# 全局资源初始化
# ==============================================
event_queue = SimpleAsyncQueue()  # 全局事件队列实例

# ==============================================
# 消费者相关功能
# ==============================================
async def event_handler(event):
    """
    事件处理器（同步执行）
    参数：
    - event: 字符串类型事件标识
    """
    if event == "sensor_trigger":
        print("Sensor Triggered!")
    elif event == "button_pressed":
        print("Button Pressed!")
    else:
        print("Unknown Event")

async def event_listener():
    """
    事件监听协程（消费者任务）
    工作流程：
    1. 持续监听事件队列
    2. 获取到事件后调用处理器
    3. 输出调试信息
    """
    while True:
        event = await event_queue.get()  # 阻塞等待事件
        await event_handler(event)       # 同步处理事件
        print("事件已处理，继续监听...")

# ==============================================
# 生产者相关功能
# ==============================================
async def simulate_events():
    """
    事件模拟器（生产者任务）
    工作流程：
    1. 延迟1秒后触发传感器事件
    2. 间隔延迟3秒后触发按钮事件
    """
    await asyncio.sleep(1)               # 初始延迟
    await event_queue.put("sensor_trigger")  # 产生传感器事件
    
    await asyncio.sleep(3)               # 事件间间隔
    await event_queue.put("button_pressed")  # 产生按钮事件

# ==============================================
# 独立后台任务
# ==============================================
async def blink(led, period_ms):
    """
    LED闪烁控制任务
    参数：
    - led: Pin对象，控制的LED引脚
    - period_ms: 闪烁周期（毫秒）
    工作流程：
    1. 开启LED
    2. 短暂保持亮起状态（period_ms）
    3. 关闭LED并等待完整周期
    """
    try:
        while True:
            led.on()                    # 点亮LED
            await asyncio.sleep_ms(period_ms)  # 保持亮起period_ms
            led.off()                   # 关闭LED
            await asyncio.sleep_ms(1000)  # 等待完整周期
            print("LED完成一次闪烁周期")
    except asyncio.CancelledError:      # 任务取消时的清理
        led.off()                       # 确保LED关闭

# ==============================================
# 主控制系统
# ==============================================
async def main():
    """
    主协程（系统入口）
    执行步骤：
    1. 初始化硬件设备
    2. 创建所有异步任务
    3. 保持主循环运行
    """
    # 硬件初始化
    led1 = Pin(25, Pin.OUT)  # 使用Pico板载LED（GPIO25）
    
    # 创建并启动所有任务
    tasks = [
        asyncio.create_task(event_listener()),  # 消费者任务
        asyncio.create_task(simulate_events()),  # 生产者任务
        asyncio.create_task(event_listener()),  # 消费者任务
        asyncio.create_task(simulate_events()),  # 生产者任务
        asyncio.create_task(blink(led1, 50))  # 独立后台任务（闪烁周期50毫秒）
    ]
    
    # 保持主任务运行（防止事件循环退出）
    while True:
        await asyncio.sleep(5)  # 每5秒唤醒一次检查状态
        print("系统运行中...")  # 心跳监测输出

# ==============================================
# 程序入口
# ==============================================
try:
    asyncio.run(main())  # 启动异步事件循环
except KeyboardInterrupt:
    print("Program stopped")  # 处理键盘中断
```