# Char

ในภาษา Rust มีประเภทข้อมูลที่ชัดเจนสำหรับตัวอักษรคือ `char` ซึ่งรองรับตัวอักษรทุกภาษา รวมถึงอีโมจิและสัญลักษณ์พิเศษ

**ตัวอย่างการใช้งาน `char`**

```rust, editable
fn main() {
    let c: char = 'ก';
    let emoji: char = '😊';
    println!("{} {}", c, emoji);
}
```

> ลองแก้โค้ดดู
>
> - ลองเปลี่ยนเครื่องหมาย `'` เป็น `"` จะเกิดอะไรขึ้น?
> - ลองเพิ่มตัวอักษรเข้าไปใน `'...'` ให้มีมากกว่า 1 ตัวอักษร จะเกิดอะไรขึ้น?

# String

ในภาษา Rust ชนิดข้อมูล `String` เป็นชนิดข้อมูลที่สามารถปรับขนาดได้และเก็บข้อมูลบน heap สามารถแก้ไขได้

> [!NOTE]
> ใน Rust `"Hello, World!"` ยังไม่ใช่ `String` แต่เป็น string slice (`&str`)

> [!NOTE]
> `"Hello, World!"` เรียกว่าตัวอักษร string (string literal) ซึ่งจัดเก็บในพื้นที่หน่วยความจำแบบ static (static memory)

**ตัวอย่างการใช้งาน `String`**

```rust, editable
fn main() {
    let s:&str = "This is my string";

    let s1:String = String::from("hello world");
    let s2:String = "hello".to_string();
    let s3:String = "world".to_owned();
    let s4:String = format!("{} {}", s2, s3);
    let s5:String = s2 + " " + &s3;

    println!("s1 is {}\ns4 is {}\ns5 is {}", s1, s4, s5);

}
```

> ลองแก้โค้ดดู
>
> - ลองเปลี่ยนชนิดของ `s1`, `s2`, `s3`, `s4`, หรือ `s5` ให้เป็น `&str` จะเกิดอะไรขึ้น?
> - ลองใช้เมธอดต่างๆ ของ `String` เช่น `push`, `pop`, หรือ `replace` เพื่อดูการเปลี่ยนแปลงของข้อมูล
