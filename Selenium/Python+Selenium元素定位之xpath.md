Selenium元素定位之xpath
----------------------
#### 例如 源码里面含有这样的：
```HTML
<a class="btn_green big w90" onclick="add_topic(this);" href="javascript:;">+ 新增一题</a>
```
* 这个是一个按钮，需要识别，并点击
```Python
add_topic = driver.find_element_by_xpath("//a[text()='+ 新增一题']")
add_topic.click()
```

* 标识相对位置，a开头
```HTML
<a href="http://www.baidu.com">百度搜索</a>
```
  > xpath写法为 //a[text()='百度搜索']
  > 或者 //a[contains(text(),"百度搜索")]


#### 类似的方法还有 

* start-with
  > 查找元素属性以某某开始的元素，如
`//input[starts-with(@name,'name2')]`     
  > 查找name属性中开始位置包含'name1'关键字的页面元素

* contains 含有
`//input[contains(@name,'topic')]`         
  > 查找name属性中包含topic关键字的页面元素
