* `Send` trait表示可以将所有权在不同线程间转移。
* `Sync` trait表示实现了`Sync`的类型可以在多个线程中被引用。
`Send`和`Sync` traits被分开设计的原因是，一个类型可能是二者之一，也可能同时是二者，还可能是二者都不是。

| Type                | `Send`                     | `Sync`                     | Reasons                                                                                     |
| ------------------- | -------------------------- | -------------------------- | ------------------------------------------------------------------------------------------- |
| `Rc<T>`             | $\times$                   | $\times$                   | 其中的clone()操作并不是原子操作。                                                                        |
| `RefCell<T>`        | $\checkmark$(if `T: Send`) | $\times$                   | `RefCell<T>`的借用检查（运行期）也不是线程安全的。                                                             |
| `Mutex<T>`          | $\checkmark$               | $\checkmark$               | 所有操作都是线程安全的，但是会有额外的性能代价（perfomance penalty）。                                                |
| `MutexGuard<'a, T>` | $\times$                   | $\checkmark$(if `T: Sync`) | `MutexGuard<'a, T>`是`Mutex<T>::lock`的返回值。其不支持`Send`是因为在一些平台的实现上，unlock操作是由实施lock操作的那个线程负责的。 |
