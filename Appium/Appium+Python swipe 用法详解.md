API
------
```Python
def swipe(self, start_x, start_y, end_x, end_y, duration=None):
    """Swipe from one point to another point, for an optional duration.

    :Args:
     - start_x - x-coordinate at which to start
     - start_y - y-coordinate at which to start
     - end_x - x-coordinate at which to stop
     - end_y - y-coordinate at which to stop
     - duration - (optional) time to take the swipe, in ms.

    :Usage:
        driver.swipe(100, 100, 100, 400)
    """
    # `swipe` is something like press-wait-move_to-release, which the server
    # will translate into the correct action
    action = TouchAction(self)
    action \
        .press(x=start_x, y=start_y) \
        .wait(ms=duration) \
        .move_to(x=end_x, y=end_y) \
        .release()
    action.perform()
    return self
```
- start_x - 滑动开始x轴坐标
- start_y - 滑动开始y轴坐标
- end_x - 滑动结束x轴偏移量
- end_y - 滑动结束y轴偏移量
- duration - (可选) 执行此次滑动时间，单位毫秒.
- 其中end_x和 end_y 为基于start_x和start_y的偏移量。
- 最终在执行中的 to_x = start_x+end_x 并非end_x
- duration 参数单位为ms（默认5毫秒） 注意1s =1000ms

### 示例：
```PYTHON
# 获取屏幕尺寸
def GetPageSize(self):
        x = self.driver.get_window_size()['width']
        y = self.driver.get_window_size()['height']
        return (x, y)
```
#### 左滑:
##### 技巧：左滑是从较大x值 --->较小x值，所以 to_x=sx+(-ex)
##### 技巧：左滑时y轴值基本无变化，所以ey=0
##### 技巧：sx的值一定大于屏幕尺寸的53%，否则虽向左滑动但不能生效切换页面
```PYTHON
def swipe_left(self):
      s = self.GetPageSize()
      sx = s[0] * 0.57
      sy = s[1] * 0.75
      ex = s[0] * 0.55
      ey = 0
      self.driver.swipe(sx, sy, -ex, ey, dt)
```
#### 右滑
##### 技巧：sx的值一定不能大于屏幕尺寸的 46%，否则虽然向右滑动但不能生效切换页面
```PYTHON
def swipe_right(self):
      s = self.GetPageSize()
      sx = s[0] * 0.43
      sy = s[1] * 0.75
      ex = s[0] * 0.54
      ey = 0
      self.driver.swipe(sx, sy, ex, ey, dt)
```
#### 上滑 （俗称上拉加载更多）
##### 技巧：上滑是从较大y值--->较小y值，所以to_y=sy+(-ey)
##### 技巧：上滑时x轴值基本无变化，所以ex = 0
##### 技巧：sx的值可s[0]范围内随意，sy和ey 需在 s[1]0.2 -- s[1]-s[1]0.4 之间取值，
##### 因为需要考虑：状态条、导航栏、底部功能栏等所占数值
```PYTHON
def swipe_up(self):
     s = self.GetPageSize()
     sx = s[0] * 0.43
     sy = s[1] * 0.45
     ex = 0
     ey = s[1] * 0.55
     self.driver.swipe(sx, sy, ex, -ey, dt）
```
#### 下滑（俗称下拉刷新）
##### 技巧：sx的值可s[0]范围内随意，sy和ey 需在 s[1]*0.2 -- s[1]-s[1]*0.4 之间取值，
##### 因为需要考虑：状态条、导航栏、底部功能栏等所占数值
```PYTHON
def swipe_down(self):
      s = self.GetPageSize()
      sx = s[0] * 0.35
      sy = s[1] * 0.45
      ex = 0
      ey = s[1] * 0.55
      self.driver.swipe(sx, sy, ex, ey, dt)
```

#### 左右上下滑屏代码实现
```PYTHON
import os
import time
from appium import webdriver
 
desired_caps ={ 'platformName': 'Android',
                'platformVersion': '6.0.1',
                'deviceName': 'KIW-AL10',
                'noReset': True,
                'appPackage': 'com.baidu.searchbox',
                'appActivity': 'com.baidu.searchbox.SplashActivity',
                'unicodeKeyboard': True,
                'resetKeyboard': True
                }
 
driver = webdriver.Remote('http://localhost:4723/wd/hub', desired_caps)#启动app
time.sleep(3) #app启动后等待3秒，方便元素加载完成
# 打印屏幕高和宽
print(driver.get_window_size())
#获取屏幕的高
x = driver.get_window_size()['width']
# 获取屏幕宽
y = driver.get_window_size()['height']
# 滑屏，大概从屏幕右边2分之一高度，往左侧滑动,滑动后显示的是 热点tab
driver.swipe(6/7*x, 1/2*y, 1/7*x, 1/2*y, 100)
time.sleep(4)
#向右滑动，显示推荐tab 内容，第五个参数，时间设置大一点，否则容易看不到滑动效果
driver.swipe(1/7*x, 1/2*y, 5/7*x, 1/2*y, 200)
time.sleep(4)
#向上滑
driver.swipe(1/2*x, 1/2*y, 1/2*x, 1/7*y, 200)
time.sleep(4)
# 向下滑动
driver.swipe(1/2*x, 1/7*y, 1/2*x, 6/7*y, 200)
```
