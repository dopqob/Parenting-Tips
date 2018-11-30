Appium输入法操作
===========================
### 一.隐藏当前键盘
* 脚本执行过程中，有时会遇到调起键盘后键盘不会自动收回，但是又遮挡住界面其他控件的情况。可以手动在输入完毕后隐藏键盘
```Python
driver.hide_keyboard()
```

### 二.切换输入法
* 使用Appium自带的Unicode键盘有时会遇到输入内容和预期不符的情况，例如预期输入为‘123456’，实际输入的确是‘12’。后发现关闭Unicode键盘，使用其他输入则不会出现该问题。
但是有时候又需要输入中文，所以可以手动根据需求在脚本执行过程中控制键盘切换。
* 使用 driver.activate_ime_engine()来切换输入法
    * 切换成appium键盘
    ```Python
    driver.activate_ime_engine('io.appium.android.ime/.UnicodeIME')
    driver.active_ime_engine
    ```

    * 切换为搜狗输入法
    ```Python
    driver.activate_ime_engine('com.sohu.inputmethod.sogouoem/.SogouIME')
    driver.active_ime_engine
    ```
### 三.检查当前输入法是否启用
```Python
self.driver.is_ime_active()
def is_ime_active(self):
"""Checks whether the device has IME service active. Returns True/False.
Android only.
"""
return self.execute(Command.IS_IME_ACTIVE, {})['value']
```
