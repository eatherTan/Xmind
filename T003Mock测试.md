# Mock测试

### 1.什么是mock测试？

mock测试就是在测试过程中，对于某些不容易构造或者不容易或者的对象，用一个虚拟的对象来创建以便测试的测试方法。

### 2.不mock会怎么样？

时间紧凑、前后端不能并行开发，迭代速度变慢

1. 后端同学开始开发，前端开始开发，想对接接口，但是接口还没有出来；

2. 后端A同学依赖后端B同学的接口
3. 当前开发的内容依赖第三方服务，开发有依赖或者是需要性能测试场景，不能去压测别人的服务

#### 此时应该怎么办？

如此，这个mock的意义就出来了，前端不再依赖后端，后端A不再依赖后端B,而是并行开发，等达到一个所有任务都完成开发的时间点， 再集成测试<前后端联调测试>，至少比在最后的情节才能提测的时间早，虽然不是绝对的早；但是也是一种不错的协同办公模式。

 怎样才能不依赖呢？ 需求评审一过，确认需求，开发识别需要做的工作，前后端约定数据结构，给出接口文档，然后开始fire，我想没有比这舒坦的工作方式了。



以下是别人的案例，先保存起来

> 需要有python基础；mock在python2中有独立的库，python3直接集成在unittest框架了。

- 场景1：接口B依赖接口A，那么只有在接口A请求正常的情况下才能正常请求接口B

```
class Demo():
    def pay(self):
        """
        这是一个由B同学开发的支付接口，未完成；
        他会返回两种状态：成功or失败
        {"msg":"success","code":0,"reason":None}
        {"msg":"failed","code":-1,"reason":"支付帐号或密码错误"}
        reason：返回失败原因
        """
        pass
    
    
    def get_status(self):
        """根据支付结果状态"""
        res=self.pay()
        try:
            if res["msg"]=="success":
                return "支付成功"
            elif res["msg"]=="failed":
                return "支付失败"
            else:
                return "未知异常"
        except:
            return "Error,服务端异常"

```

- 根据Demo模块设计单元测试用例

```
import unittest
from unittest import mock

class TestPayStatus(unittest.TestCase):
    """单元测试用例"""
    
    # 实例化对象
    demo=Demo()
    
    def test_success(self):
        """测试支付成功场景"""
        # 模拟支付成功响应
        self.demo.pay=mock.Mock(return_value={"msg":"success","code":0,"reason":None})
        status=self.demo.get_status()
        # 断言
        self.assertEqual(status, "支付成功")
        
    def test_failed(self):
        """测试支付失败场景"""
        # 模拟支付失败响应
        self.demo.pay=mock.Mock(return_value={"msg":"failed","code":-1,"reason":"支付帐号或密码错误"})
        status=self.demo.get_status()
        # 断言
        self.assertEqual(status, "支付失败")
    
    def test_error(self):
        """测试异常场景"""
        # 模拟支付失败响应
        self.demo.pay=mock.Mock(return_value={"msg":"error","code":-2,"reason":"未知异常"})
        status=self.demo.get_status()
        # 断言
        self.assertEqual(status, "未知异常")
    
    def test_error2(self):
        """测试服务端异常场景"""
        # 模拟支付失败响应
        self.demo.pay=mock.Mock(return_value={})
        status=self.demo.get_status()
        # 断言
        self.assertEqual(status, "Error,服务端异常")

```

- 上面的案例，是不是很简单，并且用例覆盖率达到100%， 欢迎大佬拍砖!!!