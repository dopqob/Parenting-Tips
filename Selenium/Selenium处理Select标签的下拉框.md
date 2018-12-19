# Selenium处理Select标签的下拉框

## 1.通过selenium.webdriver.support.ui的Select进行定位

```Python
from selenium.webdriver.support.ui import Select

# 通过index进行选择
Select(driver.find_element_by_id("gender")).select_by_index(1)

# 通过value进行选择
Select(driver.find_element_by_id("gender")).select_by_value("2")

# 通过选项文字进行选择
Select(driver.find_element_by_id("gender")).select_by_visible_text("Male")
```
**`注：Select only works on elements（Select只对标签的下拉菜单有效).`**

```Html
<select id="status" class="form-control valid" onchange="" name="status">
    <option value=""></option>
    <option value="0">未审核</option>
    <option value="1">初审通过</option>
    <option value="2">复审通过</option>
    <option value="3">审核不通过</option>
</select>
```
* 查看Selenium代码select.py文件的实现：
```Python
class Select:
    def __init__(self, webelement):
        """
        Constructor. A check is made that the given element is, indeed, a SELECT tag. If it is not,
        then an UnexpectedTagNameException is thrown.

        :Args:
         - webelement - element SELECT element to wrap
        
        Example:
            from selenium.webdriver.support.ui import Select \n
            Select(driver.find_element_by_tag_name("select")).select_by_index(2)
        """
        if webelement.tag_name.lower() != "select":
            raise UnexpectedTagNameException(
                "Select only works on <select> elements, not on <%s>" % 
                webelement.tag_name)
        self._el = webelement
        multi = self._el.get_attribute("multiple")
        self.is_multiple = multi and multi != "false"
```

* 查看Select类的实现需要一个元素的定位。并且Example中给了例句：
```Python
Select(driver.find_element_by_tag_name("select")).select_by_index(2)
```
* 继续查看select_by_index() 方法的使用并符合上面的给出的下拉框的要求，因为它要求下拉框的选项必须要有index属性，例如index="1"
```Python
def select_by_index(self, index):
      """Select the option at the given index. This is done by examing the "index" attribute of an
         element, and not merely by counting.

         :Args:
          - index - The option at this index will be selected 
         """
      match = str(index)
      matched = False
      for opt in self.options:
          if opt.get_attribute("index") == match:
              self._setSelected(opt)
              if not self.is_multiple:
                  return
              matched = True
      if not matched:
          raise NoSuchElementException("Could not locate element with index %d" % index)
```
* 继续查看select_by_value() 方法符合我们的需求，它用于选取<option>标签的value值
```Python
def select_by_value(self, value):
    """Select all options that have a value matching the argument. That is, when given "foo" this
       would select an option like:

       <option value="foo">Bar</option>

       :Args:
        - value - The value to match against
       """
    css = "option[value =%s]" % self._escapeString(value)
    opts = self._el.find_elements(By.CSS_SELECTOR, css)
    matched = False
    for opt in opts:
        self._setSelected(opt)
        if not self.is_multiple:
            return
        matched = True
    if not matched:
        raise NoSuchElementException("Cannot locate option with value: %s" % value)
```
* 最终，可以通过下面有实现选择下拉框的选项
```Python
from selenium.webdriver.support.select import Select

sel = driver.find_element_by_xpath("//select[@id='status']")
Select(sel).select_by_value('0')  #未审核
Select(sel).select_by_value('1')  #初审通过
Select(sel).select_by_value('2')  #复审通过
Select(sel).select_by_value('3')  #审核不通过
```

## 2.定位非标签的下拉菜单
```Python
# 先定位到下拉菜单 
drop_down = driver.find_element_by_css_selector("div#select2_container > ul") 
# 再对下拉菜单中的选项进行选择 
drop_down.find_element_by_id("li2_input_2").click() 
```
`**注：也可以用此方法定位标签的下拉菜单.**`
