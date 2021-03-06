#  [python设计模式之责任链模式](http://dongweiming.github.io/python-chain.html)

## 责任链模式

比如我们还在读书的时候，考试的分数都是几个档次，比如90-100分，80-90分，好吧我想做一个根据分数打印你的学习成绩的反馈，
比如90-100就是A+，80-90就是A，70-80就是B+… 当然你可以用很多种方法实现，我这里就来实现一个Chain模式:用一系列的类来响应，
但只有遇到适合处理它的类才会处理，类似与case和switch的作用

## python的例子

    
    
    class BaseHandler:
        # 它起到了链的作用
        def successor(self, successor):
            self.successor = successor
    
    class ScoreHandler1(BaseHandler):
        def handle(self, request):
            if request > 90 and request <= 100:
                return "A+"
            else:
                # 否则传给下一个链，下同，但是我是要return回结果的
                return self.successor.handle(request)
    
    class ScoreHandler2(BaseHandler):
        def handle(self, request):
            if request > 80 and request <= 90:
                return "A"
            else:
                return self.successor.handle(request)
    
    class ScoreHandler3(BaseHandler):
        def handle(self, request):
            if request > 70 and request <= 80:
                return "B+"
            else:
                return "unsatisfactory result"
    
    class Client:
        def __init__(self):
            h1 = ScoreHandler1()
            h2 = ScoreHandler2()
            h3 = ScoreHandler3()
            # 注意这个顺序，h3包含一个类似于default结果的东西，是要放在最后的，其他的顺序是无所谓的，比如h1和h2
            h1.successor(h2)
            h2.successor(h3)
    
            requests =  {'zhangsan': 78,
                        'lisi': 98,
                        'wangwu': 82,
                        'zhaoliu': 60}
            for name, score in requests.iteritems():
                print '{} is {}'.format(name, h1.handle(score))
    
    if __name__== "__main__":
        client = Client()
    


