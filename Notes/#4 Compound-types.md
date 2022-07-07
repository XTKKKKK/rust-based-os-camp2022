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

   上述代码中，my_name是一个切片的引用，这与greet函数中形参的类型不符。因此会报错。

   > let s: &str = "Hello, world!";
   > 等同于
   >
   > let s = "Hello, world!";

2. UTF-8字符操作

   字符串操作以字节形式对待切片，这在UTF-8中会有意想不到的问题。操作字符需要用`for in &str.chars()`来遍历

3. 取切片的方式：

   ```rust
   let hello = &s[0..5];
   ```

4. 隐式转换

   ```rust
   fn main() {
       let mut s = String::from("hello world");
   
   // 这里, &s 是 `&String` 类型，但是 `first_word` 函数需要的是 `&str` 类型。尽管两个类型不一样，但是代码仍然可以工作，原因是 `&String` 会被隐式地转换成 `&str` 类型，如果大家想要知道更多，可以看看 Deref 章节: https://course.rs/advance/smart-pointer/deref.html
       let word = first_word(&s);
   
       s.clear(); // error!
   
       println!("the first word is: {}", word);
   }
   fn first_word(s: &str) -> &str {
       &s[..1]
   }
   ```



#### 元组

只有长度在12以内（含12）的元组才有Show特征，才能println

#### 结构体

