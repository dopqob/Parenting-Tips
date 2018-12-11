使用unittest进行测试，如果是需要实现上百个测试用例，把它们全部写在一个test.py文件中，文件会越来越臃肿，后期维护麻烦
此时可以将这些用例按照测试功能进行拆分，分散到不同的测试文件中:

### testadd.py
```Python
from calculator import Math
import unittest


class TestAdd(unittest.TestCase):
    def setUp(self):
        print("test case start")

    def tearDown(self):
        print("test case end")

    def test_add(self):
        j = Math(2, 3)
        self.assertEqual(j.add(), 5)

    def test_add2(self):
        j = Math(41, 76)
        self.assertEqual(j.add(), 117)

if __name__ == '__main__':
    unittest.main()
```

### testsub.py
```Python
from calculator import Math
import unittest


class TestSub(unittest.TestCase):
    def setUp(self):
        print("test case start")

    def tearDown(self):
        print("test case end")

    def test_sub(self):
        j = Math(2, 3)
        self.assertEqual(j.sub(), -1)

    def test_sub2(self):
        j = Math(81, 76)
        self.assertEqual(j.sub(), 5)


if __name__ == '__main__':
    unittest.main()
```

### result.py
```Python
import unittest

test_dir = './'
discover = unittest.defaultTestLoader.discover(test_dir, pattern='test*.py')

if __name__ == '__main__':
    runner = unittest.TextTestRunner()
    runner.run(discover)
```

### TestLoader
该类根据各种标准加载测试用例，并将它们返回给测试套件。正常情况下，不需要创建这个类的实例。unittest提供了可以共享的defaultTestLoader类，可以使用其子类和方法创建实例，discover()就是其中之一
```Python
discover(start_dir,pattern='test*.py',top_level_dir=None)
```
##### 找到指定目录下所有测试模块，并可递归查到子目录下的测试模块，只有匹配到文件名才能被加载。如果启动的不是顶层目录，那么顶层目录必须单独指定
  - start_dir：要测试的模块名或测试用例目录
  - pattern='test*.py'：表示用例文件名的匹配原则。此处匹配文件名以“test”开头的“.py”类型的文件，幸好“*”表示任意多个字符
  - top_level_dir=None：测试模块的顶层目录，如果没有顶层目录，默认为None
