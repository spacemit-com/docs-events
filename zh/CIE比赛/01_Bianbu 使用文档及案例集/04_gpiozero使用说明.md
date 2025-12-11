> 本文列出已在Muse Pi Pro上调试过的GPIO模块及使用说明，可供开发者参考。

# 1. 环境搭建

**（1）安装系统依赖**

```
sudo apt update
sudo apt install -y libopenblas-dev \
		python3-dev \
		python3-venv \
		libcjson-dev \
		libasound2-dev
```

**（2）创建python虚拟环境**

设置spacemit python pip源：

```
pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple
pip config set global.extra-index-url https://git.spacemit.com/api/v4/projects/33/packages/pypi/simple
```

创建虚拟环境：

```
python3 -m venv ~/gpioenv
source ~/gpioenv/bin/activate
```

安装python依赖：

```
pip install gpiozero pyserial
```

安装成功则显示：

```
Installing collected packages: pyserial, lgpio, setuptools, colorzero, gpiozero
Successfully installed colorzero-2.0 gpiozero-2.0.5 lgpio-0.2.2.0 pyserial-3.5 setuptools-78.1.0
```

注意 gpiozero 的版本必须 >= 2.0.5

# 2.设备引脚布局

进迭时空官方参考链接：[MUSE Pi Pro 用户使用指南](https://developer.spacemit.com/documentation?token=EIk1wVY9NinD95kMsw0cFM89npd)

Muse Pi Pro开发板引脚图如下所示：

![](images/muse-pi-pro-pins.png)

在编程时，引脚编号请使用GPIO后面的数字。

也可以在终端输入以下命令便捷查看：

```
sudo chmod 666 /dev/gpiochip0
source ~/gpioenv/bin/activate
pinout
```

输出：

<img src="images/pinout-show.png" style="zoom:80%;" />

# 3.从Python使用GPIO

## 3.1 LED控制

### 硬件连接

<img src="images/ledconnect.png" style="zoom: 67%;" />

绿色杜邦线连接到GPIO 70控制引脚。

通过N型MOS管控制LED灯，从左到右的引脚为 G、D、S。

### 设备测试

以下示例代码控制连接到GPIO70的LED：

```python
from gpiozero.pins.lgpio import LGPIOFactory
from gpiozero import Device
Device.pin_factory = LGPIOFactory(chip=0)

from gpiozero import LED
import time

pin_number = 70

led1 = LED(pin_number)

try:
    while True:
        # 设置 GPIO 70 为高电平
        led1.on()
        print(f"GPIO {pin_number} ON")
        time.sleep(1)  # 等待 1 秒

        # 设置 GPIO 70 为低电平
        led1.off()
        print(f"GPIO {pin_number} OFF")
        time.sleep(1)  # 等待 1 秒

except KeyboardInterrupt:
    # 捕捉 Ctrl+C 并退出
    print("Exiting")

led1.close()
```

新建一个 led.py 文件并粘贴上面的代码。

```
sudo chmod 666 /dev/gpiochip0
source ~/gpioenv/bin/activate
python led.py
```

运行脚本之后，LED灯将以 1s 的间隔时间闪烁。

> [!NOTE]
>
> 开发板上的GPIO的驱动能力通常较弱，因此不建议将LED等功率器件直接接在引脚上。建议您给GPIO引脚加上一个大于10千欧的上拉电阻，然后使用三极管或者MOS管控制LED等设备的通断。

### 代码解释

当您在进迭时空的开发板上使用gpiozero库时，建议在程序的开头加上以下代码：

```python
from gpiozero.pins.lgpio import LGPIOFactory
from gpiozero import Device
Device.pin_factory = LGPIOFactory(chip=0) # 显式指定/dev/gpiochip0
```

这段代码显式地指定了gpiozero库使用lgpio库作为底层的引脚工厂，lgpio是针对linux更底层的GPIO控制库，虽然gpiozero默认使用lgpio，但为了保证gpiozero在SpacemiT开发板上的正常工作，建议显示地指定引脚工厂。

当您尝试移植树莓派官方例程时，建议您在其示例代码的开头加上上述三行代码，同时注意引脚编号的差异。

## 3.2 PWMLED

硬件连接方式与3.1节相同。

以下示例代码控制连接到GPIO70的LED，并输出PWM实现一个呼吸灯：

```python
from gpiozero.pins.lgpio import LGPIOFactory
from gpiozero import Device
Device.pin_factory = LGPIOFactory(chip=0)
from gpiozero import PWMLED
from signal import pause

pin_number = 70

print(f"PWM {pin_number}")
led = PWMLED(pin_number)

led.pulse()

pause()
```

通过IDE或者终端执行上述python脚本， 您应该可以看到LED灯逐渐由亮变暗然后由暗变亮。

## 3.3 读取按钮状态

### 硬件连接

<img src="images/button.png" style="zoom: 50%;" />

### 设备测试

以下示例代码读取连接到GPIO70的按钮的状态：

```python
from gpiozero.pins.lgpio import LGPIOFactory
from gpiozero import Device
Device.pin_factory = LGPIOFactory(chip=0)

from gpiozero import Button
from signal import pause

pin_number = 70

print(f"Button {pin_number}")

def say_hello():
    print("Hello! Pressed")

def say_goodbye():
    print("Goodbye! Released")

button = Button(pin_number)

button.when_pressed = say_hello
button.when_released = say_goodbye

pause()
```

通过IDE或者终端执行上述python脚本，反复按下按键，您应该可以看到如下输出：

```shell
➜  testmy python3 testbutton.py
Button 70
Hello! Pressed
Goodbye! Released
Hello! Pressed
Goodbye! Released
```

关于 GPIO 的更多信息，请参考：https://bianbu-linux.spacemit.com/device/peripheral_driver/GPIO