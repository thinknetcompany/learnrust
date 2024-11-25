# Enum

Enum ในภาษา Rust เป็นการกำหนดประเภทข้อมูลที่สามารถมีหลายรูปแบบ แต่ละรูปแบบของ `enum` สามารถเก็บข้อมูลได้แตกต่างกัน

**ตัวอย่างการใช้งาน Enum**

```rust, editable
enum DataType {
    Type1,
    Type2 { x: i32, y: i32 },
    Type3(String),
    Type4(u8, u8, u8),
}

fn main() {
    let data1 = DataType::Type1;
    let data2 = DataType::Type2 { x: 0, y: -4 };
    let data3 = DataType::Type3("hello world".to_string());
    let data4 = DataType::Type4(42, 123, 255);
}
```

> [!NOTE]
> เทียบได้กับ Typescript
>
> ```
>  type DataType = 
>  | { type: "type1" } 
>  | { type: "type2", x: number, y: number } 
>  | { type: "type3", text: string } 
>  | { type: "type4", r: number, g: number, b: number }
>
> const data1 = { type: "type1" }
> const data2 = { type: "type2", x: 0, y: -4 }
> const data3 = { type: "type3", text: "hello world" }
> const data4 = { type: "type4", r: 42, g: 123, b: 255 }
> ```

## การ Implement ฟังก์ชันให้ Enum

ในภาษา Rust เราสามารถ implement ฟังก์ชันให้กับ `enum` ได้เช่นเดียวกับ `struct`

**ตัวอย่างการ Implement ฟังก์ชันให้ Enum**

```rust, editable
enum DataType {
    Type1,
    Type2,
}

impl DataType {
    fn something() {
        println!("do something")
    }
}

fn main() {
    let data = DataType::Type1;
    data.something();
}
```

# Option<T>

ในภาษา Rust ไม่มีสิ่งที่เรียกว่า `null` เหมือนภาษาอื่นๆ แต่จะใช้ enum ที่ชื่อ `Option<T>` ในการจัดการค่าที่เป็น Optional

> [!NOTE]
> `Option<T>` คือ `enum` ที่สามารถเป็นได้ 2 อย่างคือ `Some(T)` หรือ `None`
>
> ```
> enum Option<T> {
>    Some(T),
>    None,   
> }
> ```

**ตัวอย่างการใช้งาน `Option<T>`**

```rust, editable
struct Something {
    nickname: Option<String>,
    name: String,
}

fn main() {
    let s1 = Something {
        nickname: Option::Some("H".to_string()),
        name: "Hello".to_string(),
    };

    let s2 = Something {
        nickname: Option::None,
        name: "Something".to_string(),
    };
}
```

> ลองแก้โค้ดดู
>
> - หากลบ `Option::` ออกจากโค้ด จะสามารถคอมไพล์ได้หรือไม่?

# Result

ในภาษา Rust ไม่มีสิ่งที่เรียกว่า try-catch เหมือนภาษาอื่นๆ แต่จะใช้ `enum` ที่ชื่อ `Result<T, E>` ในการจัดการกับสถานการณ์ที่อาจล้มเหลว

> [!NOTE]
> `Result<T, E>` คือ `enum` ที่สามารถมีค่าได้ 2 อย่างคือ `Ok(T)` หรือ `Err(E)`
>
> ```
> enum Result<T, E> {
>    Ok(T),
>    Err(E),   
> }
> ```

**ตัวอย่างฟังก์ชันที่มีการคืนค่า `Result<T, E>`**

```
use std::fs::File;
use std::io::Error;

fn open_file(filename: &str) -> Result<File, Error> {
    File::open(filename)
}

fn main() {
    match open_file("myfile.txt") {
        Ok(file) => println!("success: {:?}", file),
        Err(e) => println!("error: {}", e),
    }
}
```
