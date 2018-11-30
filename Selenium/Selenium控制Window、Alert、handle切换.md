# Selenium控制窗口、弹窗、多窗口切换
### 控制窗口大小
  * 最大化窗口
  ```Python
  driver.maximize_window()
  ```
  
  * 设置窗口的宽、高
  ```Python
  driver.set_window_size(1200, 500)
  ```
  
### Alert弹窗的处理
  ```Python
  a1 = driver.switch_to.alert   # 旧写法
  a1 = driver.switch_to_alert() # 新写法
  a1.accept   # 表示点击确认按钮
  a1.dismiss  # 表示点击取消按钮
  a1.sendkeys() # 表示输入内容
  a1.text     # 获取弹窗的文本内容
  ```

### 定位下拉弹框
  ```Python
  from selenium.webdriver.support.ui import Select
  s = Select(driver.find_element_by_id('xx'))
  s.select_by_index()     # 索引值
  s.select_by_value()     # value值
  s.select_by_visible_text()  # 选项文本
  ```
  
### 多窗口切换处理
  ```Python
  driver.current_window_handle    # 获取当前窗口句柄
  driver.window_handles           # 获取所有窗口句柄
  driver.switch_to.window()       # 切换到指定窗口
  ```
