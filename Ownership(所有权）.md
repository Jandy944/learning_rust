# Move(借用)
## move = shallow copy + invalidating the first value.
"借用"意味着“浅拷贝”和“之前的值无效”两个概念的叠加。

## `Copy`Trait 
对于实现了`Copy`trait的类，默认的赋值操作将是拷贝，而不是移动。

`Copy` trait 表示的是这个类可以被完全地分配在栈（Stack）上。对于这样的类，浅拷贝和深拷贝的是一样的，或者说，它没有深拷贝的概念，因为他没有指向堆内存的指针，没有数据存在堆上。而自身的数据，每一个条目都可以被一个指针长度的位数（bits）所容纳，因此直接拷贝这个数据就够了，不必把这个数据存储在堆上，然后用一个指针去指向它（当然实际上你可以这么做，但没必要，因为效率更低）。

因为`Copy` trait表示的该类全部都位于stack上，而位于heap上的类有个trait`Drop`用于表示释放heap上资源的操作，因此`Copy`trait 和`Drop`trait是互斥的。

# [[References（引用）]]
一种无须改变变量所有权就可以使用变量值的特性（feature）。