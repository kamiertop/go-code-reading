---
description: 单例化利器Once
---

# 😀 Once

#### 结构体定义

```go
type Once struct {
    // 标记Do方法是否已经执行过, 并且是一个原子变量, 可以并发安全地检查和更新done的状态
    done atomic.Uint32
    // 互斥锁, 在slow path中调用, 确保只有一个Do调用可以执行f
    m Mutex
}
```

* 将`done`作为第一个字段，done的地址就是这个结构体变量的地址，同时也一定是被调用最频繁的字段，相比作为第二个字段，可以减少一次地址计算

`Do`方法

> 方法比较简单，这里直接粘贴代码

```go
func (o *Once) Do(f func()) {
    if o.done.Load() == 0 {
        o.doSlow(f)
    }
}
```

工作原理





