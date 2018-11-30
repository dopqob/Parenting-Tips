1.通过selenium.webdriver.support.ui的Select进行定位
    from selenium.webdriver.support.ui import Select

    # 通过index进行选择
    Select(driver.find_element_by_id("gender")).select_by_index(1)
    # 通过value进行选择
    Select(driver.find_element_by_id("gender")).select_by_value("2")
    # 通过选项文字进行选择
    Select(driver.find_element_by_id("gender")).select_by_visible_text("Male")
注：Select only works on <select> elements（Select只对<select>标签的下拉菜单有效).

2.定位非<select>标签的下拉菜单
    # 先定位到下拉菜单
    drop_down = driver.find_element_by_css_selector("div#select2_container > ul")
    # 再对下拉菜单中的选项进行选择
    drop_down.find_element_by_id("li2_input_2").click()
注：也可以用此方法定位<select>标签的下拉菜单.
