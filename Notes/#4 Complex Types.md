### 字符串和切片

#### 切片

切片允许你引用集合中部分连续的元素序列，而不是引用整个集合。

**切片的几个注意点**

1. 字符串字面量是切片

   ```rust
   fn main() {
     let my_name = "Pascal";
     greet(my_name);
   }
   
   fn greet(name: String) {
     println!("Hello, {}!", name);
   }
   ```

   上述代码中，my_name是一个切片的引用，这与greet函数中形参的类型不符。