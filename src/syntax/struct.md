# Struct

ในภาษา Rust, `struct` คล้ายกับ `Object` ใน JavaScript ซึ่งใช้เก็บข้อมูลหลายประเภทเข้าด้วยกัน แต่ต่างจาก JavaScript ตรงที่ Rust เป็นภาษาที่มีชนิดข้อมูลทุก `field` ใน `struct` ต้องระบุชนิดข้อมูลอย่างชัดเจน

**ตัวอย่างการใช้งาน `struct`**

```rust, editable
struct People {
    first_name: String,
    last_name: String,
    age: u8,
}

fn main() {
    let mut people = People {
        first_name: "name".to_string(),
        last_name: "lastname".to_string(),
        age: 50,
    };

    people.age += 1;

    let first_name = people.first_name;

    println!("{} {} {}", first_name, people.last_name, people.age);
}
```

> [!NOTE]
> นอกจาก `struct` แบบมีชื่อ `field`, Rust ยังมี tuple struct และ unit struct
>
> ```
>  struct Color(u8, u8, u8);
>  struct Unit;
> ```

## การ Implement ฟังก์ชันให้ `struct`

สามารถทำได้โดยใช้ `impl` keyword เพื่อให้ `struct` มีฟังก์ชันที่สามารถเรียกใช้งานได้

**ตัวอย่างการ Implement ฟังก์ชันให้ `struct`**

```rust, editable
struct Unit;

fn main() {
    let a = Unit;

    Unit::hello();
    a.s_hello();
}

impl Unit {
    fn hello() {
        println!("hello");
    }

    fn s_hello(&self) {
        println!("s_hello");
    }
}
```

> [!NOTE]
> เทียบได้กับ Typescript
>
> ```
> class Unit {
>
>     constructor() {
>     }
>
>     static hello() {
>         console.log("hello")
>     }
>
>     s_hello() {
>         console.log("s_hello")
>     }
>    
> }
>
> const a = new Unit();
>
> Unit.hello();
> a.s_hello();
> ```
