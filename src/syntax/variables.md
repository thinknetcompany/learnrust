# let

`let` ใน Rust ใช้สำหรับการประกาศตัวแปร คล้ายกับ `let` หรือ `const` ใน JavaScript ซึ่งช่วยให้สามารถสร้างตัวแปรใหม่ได้

**ตัวอย่างการใช้ `let`**

```rust, editable
fn main() {
    let a:i32 = 5;
    println!("{}", a);
}
```

> [!NOTE]
> `:i32` คือการบอกว่า `a` มีชนิดข้อมูลเป็น `i32`

> [!NOTE]
> การเลือกชนิดข้อมูลอย่างเหมาะสม จะช่วยให้การใช้งานหน่วยความจำมีประสิทธิภาพมากขึ้น

> [!NOTE]
> เราสามารถประกาศตัวแปรชื่อซ้ำได้ ซึ่งจะทำให้ไม่สามารถเข้าถึงข้อมูลของตัวแปรก่อนหน้า (shadowing) ตัวอย่าง:
>
> ```rust, editable
> fn main() {
>     let a:i32 = 5;
>     let a:char = 'A';
>     println!("{}", a);
> }
> ```

# mut

ตัวแปรที่ประกาศด้วย `let` จะเป็น immutable (ไม่สามารถเปลี่ยนแปลงค่าได้) โดยค่าเริ่มต้น ถ้าต้องการให้ตัวแปรสามารถเปลี่ยนแปลงค่าได้ จะต้องใช้ `mut`

**ตัวอย่าง**

```
fn main() {
    let a:i32 = 5;
    a += 1;
    println!("{}", a);
}
```

```
Compiling playground v0.0.1 (/playground)
error[E0384]: cannot assign twice to immutable variable `a`
 --> src/main.rs:3:5
  |
2 |     let a:i32 = 5;
  |         - first assignment to `a`
3 |     a += 1;
  |     ^^^^^^ cannot assign twice to immutable variable
  |
help: consider making this binding mutable
  |
2 |     let mut a:i32 = 5;
  |         +++

For more information about this error, try `rustc --explain E0384`.
error: could not compile `playground` (bin "playground") due to 1 previous error
```

จะเกิด Error เพราะ `a` เป็น immutable ซึ่งเราสามารถแก้ได้โดยการใช้ `mut` ตามตัวอย่างข้างล่าง

```rust, editable
fn main() {
    let mut a:i32 = 5;
    a += 1;
    println!("{}", a);
}
```

# const

`const` ใน Rust ใช้สำหรับการประกาศค่าคงที่ที่ไม่สามารถเปลี่ยนแปลงได้ โดยต้องระบุชนิดข้อมูลอย่างชัดเจน และค่าต้องถูกคำนวณใน compile-time ซึ่งทำให้ `const` ใน Rust มีลักษณะและข้อกำหนดที่ชัดเจนกว่าการใช้ `const` ใน JavaScript

**ตัวอย่างการใช้งาน `const`**

```rust, editable
const GLOBAL:i32 = 10;

fn add(a:i32, b:i32) -> i32 {
    a + b
}

fn main() {
    const CONST:i32 = 5;
    println!("{}", GLOBAL);
    println!("{}", CONST)
}
```

> [!NOTE]
> ฟังก์ชัน `add` รับพารามิเตอร์ `a` และ `b` แล้วคืนค่า `a + b`

> ลองแก้ Code ดู
>
> - เราใช้ `let` แทน `const` สำหรับตัวแปร `GLOBAL` ได้ไหม?
> - เราประกาศ `const CONST: i32 = add(2, 3);` ได้ไหม?

# static

`static` ใน Rust ใช้สำหรับประกาศค่าหรือข้อมูลที่มีอายุการใช้งานตลอดทั้งโปรแกรม ซึ่งหมายความว่าค่าหรือข้อมูลที่ประกาศด้วย `static` จะถูกจัดเก็บในพื้นที่หน่วยความจำแบบ static (static memory) และจะไม่ถูกลบออกเมื่อออกจาก scope ที่ประกาศ

ตัวอย่างการใช้งาน `static`

```rust, editable
static mut GLOBAL:i32 = 10;

fn something() -> &'static str {
    "Hello, World!"
}

fn main() {
    let a: &str = something();
    println!("{}", a);
}
```

> [!NOTE]
> การใช้งาน หรือแก้ไขค่าตัวแปรที่เป็น `static mut` จะต้องทำใน `unsafe` block ซึ่งจะไม่อยู่ใน scope ของ Workshop EDM - Rust 2024

> [!NOTE]
> `'` ใน Rust ใช้สำหรับบอก **lifetime**

> ลองแก้โค้ดดู
>
> - เราประกาศ `const mut GLOBAL:i32 = 10;` ได้ไหม?
> - ถ้าเราเปลี่ยน
>
> ```
> fn something() -> &'static str {
>     "Hello, World!"
> }
> ```
>
> เป็น
>
> ```
> fn something() -> &str {
>     "Hello, World!"
> }
> ```
>
> จะเกิดอะไรขึ้น?
