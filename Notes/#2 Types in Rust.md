### 4.1 数值类型

#### 整数

| 8 位       | `i8`    | `u8`    |
| ---------- | ------- | ------- |
| 16 位      | `i16`   | `u16`   |
| 32 位      | `i32`   | `u32`   |
| 64 位      | `i64`   | `u64`   |
| 128 位     | `i128`  | `u128`  |
| 视架构而定 | `isize` | `usize` |

要显式处理可能的溢出，可以使用标准库针对原始数字类型提供的这些方法：

- 使用 `wrapping_*` 方法在所有模式下都按照补码循环溢出规则处理，例如 `wrapping_add`
- 如果使用 `checked_*` 方法时发生溢出，则返回 `None` 值
- 使用 `overflowing_*` 方法返回该值和一个指示是否存在溢出的布尔值
- 使用 `saturating_*` 方法使值达到最小值或最大值

例如：

```rust
fn main() {
    assert!(1u8 - 2 == 255);
}

   Compiling playground v0.0.1 (/playground)
error[E0600]: cannot apply unary operator `-` to type `u8`
 --> src/main.rs:4:24
  |
4 |     assert!(1u8 - 2 == -1);
  |                        ^^
  |                        |
  |                        cannot apply unary operator `-`
  |                        help: you may have meant the maximum value of `u8`: `u8::MAX`
  |
  = note: unsigned values cannot be negated

For more information about this error, try `rustc --explain E0600`.
error: could not compile `playground` due to previous error
```

此时可以

```rust
fn main() {
    assert!(1u8.wrapping_sub(2) == 255);
}
```



### 4.2 单元类型

单元类型是且唯一是 `()`，本质是一个零长度的元组。这是一个很特殊的值。

`fn main()` 或是 `println!()`实质上有一个特殊返回值，就是`()`，这和其他语言很不一样。JS/C中返回类型是void。而没有返回值的函数在Rust有单独的定义：`发散函数( diverge function )`。

单元类型可以作为`map`的值，仅用作占位使用，不占用任何内存。

表达式如果不返回任何值，会隐式地返回一个 [`()`](https://course.rs/basic/base-type/char-bool.html#单元类型) 。



### 4.3 函数返回

`fn main() -> ()` == `fn main()`

> - 函数没有返回值，那么返回一个 `()`
> - 通过 `;` 结尾的表达式返回一个 `()`

当用 `!` 作函数返回类型的时候，表示该函数永不返回( diverge function )，特别的，这种语法往往用做会导致程序崩溃的函数：

```rust
fn main() {
    never_return_panic();
  	never_return_loop();  
}

fn never_return_painc() -> ! {
    panic!("永不返回")
    //unimplemented!()

}
fn never_return_loop() -> ! {
  	loop{}
}
```









