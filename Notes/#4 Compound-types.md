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

##### 元组结构体

元组结构体看起来跟元组很像，但是它拥有一个结构体的名称，该名称可以赋予它一定的意义。由于它并不关心内部数据到底是什么名称，因此此时元组结构体就非常适合。

```rust

struct Color(i32, i32, i32);
struct Point(i32, i32, i32);
fn main() {
    let v = Point(0, 127, 255);
    check_color(v);
}   

fn check_color(p: Point) {
    let Point(x, _, _) = p;
		//let (x, _, _) = p; 错误，元组结构体需
    assert_eq!(x, 0);
    assert_eq!(p.1, 127);
    assert_eq!(p.2, 255);
 }

```



#### 枚举类型

Rust的枚举类型是一个很神奇的东西，相较于其他语言的单一映射，Rust可以给枚举成员关联上任意数据类型。

由于每个结构体都有自己的类型，因此我们无法在需要同一类型的地方进行使用，例如某个函数它的功能是接受消息并进行发送，那么用枚举的方式，就可以接收不同的消息，但是用结构体，该函数无法接受 4 个不同的结构体作为参数。

而且从代码规范角度来看，枚举的实现更简洁，代码内聚性更强，不像结构体的实现，分散在各个地方。

文档给了一个很好的例子：

```rust

fn new (stream: TcpStream) {
  let mut s = stream;
  if tls {
    s = negotiate_tls(stream)
  }

  // websocket是一个WebSocket<TcpStream>或者
  //   WebSocket<native_tls::TlsStream<TcpStream>>类型
  websocket = WebSocket::from_raw_socket(
    stream, ......);
  some_next_mission(websocket)
}

enum Websocket {
  Tcp(Websocket<TcpStream>),
  Tls(Websocket<native_tls::TlsStream<TcpStream>>),
}

fn some_next_mission(socket: WebSocket) {
  ...
}
```



#### Option 与 null

Rust为了安全性，抛弃了null，改用option枚举变量来表述这种结果。

```rust
enum optipon<T> {
	Some(T),
	None,
}

let some_number = Some(5);
let some_string = Some("a string");
// None表述的含义和null一致（空值）
let absent_number: Option<i32> = None;
```

> Q:`Option<T>` 为什么就比空值要好呢？
>
> A: 对于`option<T>`值，无法和`T`类型直接运算（类型不同），在使用之前[强制开发者转换为T类型后](https://doc.rust-lang.org/std/option/enum.Option.html)才能使用，防止意外取到null导致的错误。



[matches!](https://doc.rust-lang.org/stable/core/macro.matches.html)

```rust
macro_rules! matches {
    ($expression:expr, $(|)? $( $pattern:pat_param )|+ $( if $guard: expr )? $(,)?) => { ... };
}
```

