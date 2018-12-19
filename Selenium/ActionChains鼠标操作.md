ActionChains鼠标操作
-------------------
#### Actionchains是Selenium里面专门处理鼠标相关的操作如：鼠标移动，鼠标按钮操作，按键和上下文菜单（鼠标右键）交互。这对于做更复杂的动作非常有用，比如悬停和拖放。

#### actionchains也可以和快捷键结合起来使用，如ctrl,shif,alt结合鼠标一起使用

#### 当你使用actionchains对象方法，行为事件是存储在actionchains对象队列。当你使用perform()，事件按顺序执行。

* 方法一：可以写一长串
    ```Python
    menu = driver.find_element_by_css_selector(".nav")

    hidden_submenu = driver.find_element_by_css_selector(".nav #submenu1")

    ActionChains(driver).move_to_element(menu).click(hidden_submenu).perform()
    ```
* 方法二：可以分几步写
    ```Python
    menu = driver.find_element_by_css_selector(".nav")

    hidden_submenu = driver.find_element_by_css_selector(".nav #submenu1")

    actions = ActionChains(driver)
    actions.move_to_element(menu)
    actions.click(hidden_submenu)
    actions.perform()
    ```
    
## 方法介绍
```Python
from selenium.webdriver.common.action_chains import ActionChains

class ActionChains(object):
    def __init__(self, driver):
        self._driver = driver
        self._actions = []

    def perform(self):
    > 执行行为事件

    def click(self, on_element=None):
    > 点击:
    > 如果参数不写，那么点击的是当前鼠标位置
    > 如果参数写定位到的元素对象element，那就是点这个元素

    def click_and_hold(self, on_element=None):
    > 鼠标左键按住某个元素
    > 如果参数不写，那么点的是当前鼠标位置
    > 如果参数写定位到的元素对象element，那就是点这个元素

    def context_click(self, on_element=None):
    > 鼠标右键点击
    > 如果参数不写，那么点的是当前鼠标位置
    > 如果参数写定位到的元素对象element，那就是点这个元素

    def double_click(self, on_element=None):
    > 双击鼠标
    > 如果参数不写，那么点的是当前鼠标位置
    > 如果参数写定位到的元素对象element，那就是点这个元素

    def drag_and_drop(self, source, target):
    > 按住源元素上的鼠标左键，然后移动到目标元素并释放鼠标按钮
    > source: 按住鼠标的元素位置
    > target: 松开鼠标的元素位置

    def drag_and_drop_by_offset(self, source, xoffset, yoffset):
    > 按住源元素上的鼠标左键，然后移动到目标偏移量并释放鼠标按钮。
    > source: 按住鼠标的元素位置
    > xoffset: X 轴的偏移量
    > yoffset: Y 轴的偏移量

    def key_down(self, value, element=None):
    > 只发送一个按键，而不释放它。只应用于修饰键（控制、alt和shift）。

    > value: 要发送的修饰符键。值在“Keys”类中定义。
    > element: 定位的元素
    > 如果element参数不写就是当前鼠标的位置

    > 举个例子，按住 ctrl+c::

        ActionChains(driver).key_down(Keys.CONTROL).send_keys('c').key_up(Keys.CONTROL).perform()

    def key_up(self, value, element=None):
    > 释放按键，配合上面的一起使用

    def move_by_offset(self, xoffset, yoffset):
    > 将鼠标移动到当前鼠标位置的偏移量

    > xoffset: X轴 作为一个正整数或负整数移动到x偏移量
    > yoffset: Y轴 偏移，作为正整数或负整数。


    def move_to_element(self, to_element):
    > 鼠标悬停
    > to_element: 定位需要悬停的元素


    def move_to_element_with_offset(self, to_element, xoffset, yoffset):
    > 通过指定元素的偏移量移动鼠标。偏移量与元素的左上角相对
    > to_element: 定位需要悬停的元素
    > xoffset: X 轴偏移量
    > yoffset: Y 轴偏移量


    def release(self, on_element=None):
    > 释放一个元素上的鼠标按钮。

    > 如果参数不写，那么是当前鼠标位置
    > 如果参数写定位到的元素对象element，那就是这个元素.

    def send_keys(self, *keys_to_send):
    > 发送到当前焦点元素
    > 要发送的按键。修饰符键常数可以在“Keys”类。
   

    def send_keys_to_element(self, element, *keys_to_send):
    > 发送到定位到的元素上
    > element: 定位的元素
    > keys_to_send: 要发送的按键。修饰符键常数可以在“Keys”类。
```        
## 举个案例
#### 1.实现Ctrl +F5 的组合键功能
```Python
from selenium import webdriver
from selenium.webdriver.common.action_chains import ActionChains
from selenium.webdriver.common.keys import Keys
import time

driver = webdriver.Firefox()
driver.get("https://www.baidu.com")
time.sleep(3)
# 实现Ctrl+F5刷新
ActionChains(driver).key_down(Keys.CONTROL).send_keys(Keys.F5).key_up(Keys.CONTROL).perform()
```
