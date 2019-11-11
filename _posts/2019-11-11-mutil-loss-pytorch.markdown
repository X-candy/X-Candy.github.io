---
layout:     post
title:      "PyTorch 多loss情况的处理"
author:     "xcandy"
header-img: "img/post-bg-2015.jpg"
tags:
    - PyTorch
---

> 本文主要讨论了多个loss下loss函数的处理方法

## 正文
在处理多任务的时候通常会涉及到多个loss函数，关于多个loss函数的反向传播可以采用两种方法：

1、Total\_Loss = Loss1 + Loss2 ，然后求取Total\_loss的反向传播

2、获取每一个loss的反向传播

### 示例代码
```
import torch
import torch.nn as nn
import torch.optim as optim

if __name__ == "__main__":
	torch.manual_seed(1)

    model = nn.Sequential(
        nn.Linear(4, 8, bias=False),
        nn.ReLU(),
        nn.Linear(8, 1, bias=False),
    )
	#requires_grad主要是能够观察x的梯度变化
    x = torch.ones(4,requires_grad=True)
    y0 = torch.tensor(7.)
    y1 = torch.tensor(4.)

    loss_fn = nn.MSELoss()
    optimizer = torch.optim.Adam(model.parameters(), lr=0.01)

    optimizer.zero_grad()
    for i in range(200):
        y = model(x)
        loss1 = loss_fn(y, y0)
        loss2 = loss_fn(y, y1)

        if 1 == 1:
            loss = loss1+loss2
            if loss.item() < 1e-5:
                print('after {%d} steps'%i)
                break
            optimizer.zero_grad()
            loss.backward()
            print(x.grad)
        else:
            if loss1.item() < 1e-5 and loss2.item() < 1e-5:
                print('after {%d} steps'%i)
                break
            optimizer.zero_grad()
            #在采用多个loss单独计算时要设置计算图保持，否则在完成一次反向传播后，pytorch会默认销毁计算图
            loss1.backward(retain_graph=True)
            loss2.backward()
            print(x.grad)

        optimizer.step()

    print('y:', y)
    for label, value in model.state_dict().items():
        print(label)
        print(value)
```



