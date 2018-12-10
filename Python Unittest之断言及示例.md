### 前言
  #### Python Unintest单元测试框架提供了一整套内置的断言方法：
  1. 如果断言失败，则抛出一个AssertionError,并标识该测试为失败状态
  2. 如果异常，则当做错误来处理
  3. 如果成功，则标识该测试为成功状态

  #### 下面我们看下在unittest框架中定义了哪几类断言方法:
  1. 基本的Boolean断言，即：要么True，要么False的验证
  2. 简单比较断言，例如比较a,b两个变量的值
  3. 复杂断言
  
### 基本断言方法
#### 基本的断言方法提供了测试结果是True还是False。所有的断言方法都有一个msg参数，如果指定msg参数的值，则将该信息作为失败的错误信息返回
  |序号|			断言方法               		|			断言描述                     |
  |---|---------------------------------------|---------------------------------------|
  |1	|assertEqual(arg1, arg2, msg=None)		|	验证arg1=arg2，不等则fail            	|
  |2  |assertNotEqual(arg1, arg2, msg=None)	|	验证arg1 != arg2, 相等则fail			|
  |3  |assertTrue(expr, msg=None)				|	验证expr是true，如果为false，则fail	|
  |4	|assertFalse(expr,msg=None)				| 	验证expr是false，如果为true，则fail 	|
  |5	|assertIs(arg1, arg2, msg=None)			| 	验证arg1、arg2是同一个对象，不是则fail	|
  |6	|assertIsNot(arg1, arg2, msg=None)		| 	验证arg1、arg2不是同一个对象，是则fail 	|
  |7	|assertIsNone(expr, msg=None)			| 	验证expr是None，不是则fail 			|
  |8	|assertIsNotNone(expr, msg=None)		| 	验证expr不是None，是则fail 			|
  |9	|assertIn(arg1, arg2, msg=None)			| 	验证arg1是arg2的子串，不是则fail 		|
  |10 |assertNotIn(arg1, arg2, msg=None)		| 	验证arg1不是arg2的子串，是则fail 		|
  |11 |assertIsInstance(obj, cls, msg=None)	| 	验证obj是cls的实例，不是则fail 		|
  |12 |assertNotIsInstance(obj, cls, msg=None)| 	验证obj不是cls的实例，是则fail			|
  
  **看一下上述断言简单的代码示例**
  
  ```Python
  import unittest
  import sys
  reload(sys)
  sys.setdefaultencoding("utf-8")

  class demoTest(unittest.TestCase):
      def test1(self):
          self.assertEqual(4 + 5,9)

      def test2(self):
          self.assertNotEqual(5 * 2,10)

      def test3(self):
          self.assertTrue(4 + 5 == 9,"The result is False")

      def test4(self):
          self.assertTrue(4 + 5 == 10,"assertion fails")

      def test5(self):
          self.assertIn(3,[1,2,3])

      def test6(self):
          self.assertNotIn(3, range(5))

  if __name__ == '__main__':
     unittest.main()
  ```

### 比较断言
  **unittest框架提供的第二种断言类型就是比较断言**<br>
  **assertAlmostEqual (first, second, places = 7, msg = None, delta = None)**<br>
  > 验证`first`约等于`second`<br>
  > palces: 指定精确到小数点后多少位，默认为7<br>

  **assertNotAlmostEqual (first, second, places, msg, delta)**<br>
  > 验证`first`不约等于`second`<br>
  > palces: 指定精确到小数点后多少位，默认为7<br>
  > 注：在上述的两个函数中，如果`delta`指定了值，则`first`和`second`之间的差值必须`≤delta`<br>

  **assertGreater (first, second, msg = None)**<br>
  > 验证`first > second`，否则`fail`<br>
  
  **assertGreaterEqual (first, second, msg = None)**<br>
  > 验证`first ≥ second`，否则`fail`<br>

  **assertLess (first, second, msg = None)**<br>
  > 验证`first < second`，否则`fail`<br>

  **assertLessEqual (first, second, msg = None)**<br>
  > 验证`first ≤ second`，否则`fail`<br>

  **assertRegexpMatches (text, regexp, msg = None)**<br>
  > 验证正则表达式`regexp`搜索==匹配==的文本text<br>
  > regexp：通常使用`re.search()`<br>

  **assertNotRegexpMatches (text, regexp, msg = None)**<br>
  > 验证正则表达式`regexp`搜索==不匹配==的文本text<br>
  > regexp：通常使用`re.search()`<br>
  **下面看下具体的示例代码:**
  ```Python
  import unittest
  import math
  import re
  import sys
  reload(sys)
  sys.setdefaultencoding("utf-8")

  class demoTest(unittest.TestCase):
     def test1(self):
        self.assertAlmostEqual(22.0/7,3.14)

     def test2(self):
        self.assertNotAlmostEqual(10.0/3,3)

     def test3(self):
        self.assertGreater(math.pi,3)

     def test4(self):
        self.assertNotRegexpMatches("Tutorials Point (I) Private Limited","Point")

  if __name__ == '__main__':
     unittest.main()
  ```
### 复杂断言
#### unittest框架提供的第三种断言类型，可以处理元组、列表、字典等更复杂的数据类型
  |序号  |			断言方法               		|			断言描述                     |
  |---|---------------------------------------|---------------------------------------|
  |1	|assertListEqual (list1, list2, msg = None)		|	验证列表list1、list2相等，不等则fail，同时报错信息返回具体的不同的地方|
  |2  |assertTupleEqual (tuple1, tuple2, msg = None)	|	验证元组tuple1、tuple2相等，不等则fail，同时报错信息返回具体的不同的地方|
  |3  |assertSetEqual (set1, set2, msg = None)|	验证集合set1、set2相等，不等则fail，同时报错信息返回具体的不同的地方|
  |4	|assertDictEqual (expected, actual, msg = None)|验证字典expected、actual相等，不等则fail，同时报错信息返回具体的不同的地方|
  
  **下面看下具体的示例代码:**
  ```Python
  import unittest
  import sys
  reload(sys)
  sys.setdefaultencoding("utf-8")

  class demoTest(unittest.TestCase):
     def test1(self):
        self.assertListEqual([2,3,4], [1,2,3,4,5])

     def test2(self):
        self.assertTupleEqual((1*2,2*2,3*2), (2,4,6))

     def test3(self):
        self.assertDictEqual({1:11,2:22},{3:33,2:22,1:11})

  if __name__ == '__main__':
     unittest.main()
  ```
