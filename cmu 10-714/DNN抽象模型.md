模型
损失函数
优化器
```python
class Optimizer:
    def __init__(self, params):
        self.params = params

    def reset_grad(self):
        for p in self.params:
            p.grad = None

    def step(self):
        raise NotImplemented()

class SGD(Optimizer):
    def __init__(self, params, lr):
        self.params = params
        self.lr = lr

    def step(self):
        for w in self.params:
            w.data = w.data - (self.lr) * w.grad.data
            
class SGDWithMomentum(Optimizer):
    def __init__(self, params, lr, b):
        self.params = params
        self.lr = lr
        self.u = [ndl.zeros_like(p) for p in params] + [0]
        self.b = b

    def step(self):
        for t in range(len(self.params)):
            self.u[t+1] = self.b * self.u[t] + (1-self.b) * self.params[t].grad.data
            self.params[t].data = self.params[t].data - (self.lr) * self.u[t+1]
```
