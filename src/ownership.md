- [Ownership \& Borrowing](#ownership--borrowing)
  - [`Ownership & Borrowing` Rules](#ownership--borrowing-rules)
    - [Ownership](#ownership)
      - [1. Each value has a single owner at a time](#1-each-value-has-a-single-owner-at-a-time)
      - [2. Ownership can be transferred (move semantics)](#2-ownership-can-be-transferred-move-semantics)
        - [Example 1 + 2:](#example-1--2)
      - [3. When the owner goes out of scope, the value is dropped](#3-when-the-owner-goes-out-of-scope-the-value-is-dropped)
        - [Example 3:](#example-3)
    - [Borrowing](#borrowing)
      - [4. Ownership can be borrowed (references)](#4-ownership-can-be-borrowed-references)
        - [Example 4:](#example-4)
      - [5. Immutable borrows allow many references at a time](#5-immutable-borrows-allow-many-references-at-a-time)
        - [Example 5:](#example-5)
      - [6. Mutable borrow allows only one reference](#6-mutable-borrow-allows-only-one-reference)
        - [Example 6:](#example-6)
      - [7. Mutable and immutable references cannot coexist](#7-mutable-and-immutable-references-cannot-coexist)
        - [Example 7:](#example-7)
      - [8. References must be valid (lifetime)](#8-references-must-be-valid-lifetime)
        - [Example 8.1: Struct with lifetime](#example-81-struct-with-lifetime)
        - [Example 8.2: Function with lifetime](#example-82-function-with-lifetime)
  - [Exercises](#exercises)
    - [1. Move Semantics](#1-move-semantics)
    - [2. Mutable Borrowing](#2-mutable-borrowing)
    - [3. Lifetime Management](#3-lifetime-management)

# Ownership & Borrowing

> [!NOTE]
> Rust สามารถจัดการหน่วยความจำได้อย่างมีประสิทธิภาพและปลอดภัยสูง
> เนื่องจากมีกฎการจัดการหน่วยความจำที่เข้มงวด เรียกว่า `Ownership & Borrowing`
>
> กฎ `Ownership & Borrowing` ทำหน้าที่หลักๆดังนี้:
>
> - ตรวจสอบ,ควบคุมการเข้าถึงหน่วยความจำ เพื่อให้แน่ใจว่าสามารถเข้าถึง, ใช้งาน
>   และคืนค่าหน่วยความจำได้อย่างปลอดภัย
> - ป้องกันการ Copy ข้อมูลใน Heap โดยไม่จำเป็น ทำให้โปรแกรมมีประสิทธิภาพมากขึ้น
> - ป้องกันการแก้ไขหน่วยความจำจากหลายที่พร้อมกัน (Race Condition)
> - คืนหน่วยความจำโดยอัตโนมัติได้อย่างปลอดภัยเมื่อตัวแปรออกจาก scope
>
> **<u>ข้อดีของระบบนี้:</u>**
>
> - ไม่จำเป็นต้องใช้ Garbage Collector เหมือนภาษาสมัยใหม่อื่นๆ
> - ไม่ต้องจัดการหน่วยความจำด้วยตนเองเหมือน C หรือ C++
> - สามารถดักจับข้อผิดพลาดที่เกี่ยวกับหน่วยความจำได้ตั้งแต่ตอน Compile Time
> - สามารถแนะนำวิธีการเขียนโปรแกรมได้อย่างมีประสิทธิภาพและปลอดภัยมากขึ้น
>   (และทำให้เกิดนิสัยของการเขียนโปรแกรมที่ดีขึ้น)
> - ลดข้อผิดพลาดที่เกี่ยวกับหน่วยความจำได้มากกว่า 80% เมื่อเทียบกับ C และ C++ เช่น:
>   - Null Pointer Dereference
>   - Double Free
>   - Use After Free
>   - Race Condition
>   - Memory Leak
>   - และอื่นๆ

## `Ownership & Borrowing` Rules

`Ownership & Borrowing` มีกฎอยู่ 8 ข้อ โดยเป็นกลุ่มของ `Ownership` 3 ข้อ และ `Borrowing`
5 ข้อ ดังนี้:

### Ownership

#### 1. Each value has a single owner at a time

> [!TIP]
>
> - เป็นรากฐานของกฎในข้ออื่นๆ
> - เพื่อให้มั่นใจว่าข้อมูลจะถูกเข้าถึงได้อย่างปลอดภัยได้จากที่เดียวเท่านั้น (Single source of truth)
> - Memory จะถูก Deallocated ได้ถูกที่ถูกเวลาได้อย่างปลอดภัย และเพื่อป้องกันการ Double Free
>   จากการมีตัวแปรหลายตัวชี้ไปยัง Memory เดียวกัน

#### 2. Ownership can be transferred (move semantics)

> [!TIP]
>
> - การย้าย Ownership ไปยังตัวแปรใหม่ จะทำให้ตัวแปรที่เป็น Owner เดิม
>   ไม่สามารถเข้าถึงข้อมูลได้อีกต่อไป

##### Example 1 + 2:

```rust, editable
fn main() {
    // s1 เป็น Owner ของ value "Hello"
    let s1 = String::from("Hello");

    // Ownership ของ "Hello" ถูกย้ายจาก s1 มายัง s2 (กฎ move semantics จากข้อ 2)
    let s2 = s1;

    // จะเกิด error เนื่องจาก value "Hello" ถูก move ownership ไปไว้ที่ s2 แล้ว
    println!("Say: {}", s1);
}
```

<img src="/images/move_semantics.svg" alt="move semantics" width="500">

รูปประกอบตัวอย่างที่ 1 + 2

#### 3. When the owner goes out of scope, the value is dropped

> [!TIP]
>
> - Scope ใน Rust หมายถึง Block `{}` ของ code ไม่ว่าจะเป็น `fn`, `if`, `loop`,
>   `for`, หรืออื่นๆ
> - สามารถการันตีได้ว่า Memory จะถูก Deallocated ได้ถูกที่ถูกเวลาได้อย่างปลอดภัยโดยอัตโนมัติ
> - ไม่จำเป็นต้องใช้ Garbage Collector หรือ Manual Deallocation
> - ทำให้ Rust มี Memory Footprint ที่น้อยมากๆ
> - สามารถป้องกันการเกิด Memory Leak, Double Free, Use After Free
>   ได้อย่างมีประสิทธิภาพสูงสุด

##### Example 3:

```rust, editable
fn main() { // ---+---> Scope fn main
    // s1 เป็น Owner ของ value "Hello"
    let s1 = String::from("Hello");
    
    // Ownership ของ "Hello" ถูกย้ายจาก s1 มายัง argument ของ fn print_string 
    print_string(s1);
    // Value "Hello" จะถูก drop ทันทีที่จบการทำงานของ fn print_string
    // โดยไม่ต้องรอจบการทำงานของ scope หลัก (fn main)

    { // ---+---> Scope block       
        let s2 = String::from("Hello");
    } // <--- s2 จะถูก drop ทันทีตรงนี้

    /* Do other stuff */
}

fn print_string(s: String) { // ---+---> Scope fn print_string
    println!("{}", s);
} // <--- Value "Hello" จะถูก drop ทันทีตรงนี้
```

### Borrowing

#### 4. Ownership can be borrowed (references)

> [!TIP]
>
> - วิธีการยืมข้อมูลจาก Owner มาชั่วคราว (Borrowing)
>   ทำได้โดยการสร้างตัวแปรใหม่ที่เป็นตัวแปรอ้างอิง (Reference หรือ `&`)
>   ซึ่งจะชี้ไปยังข้อมูลที่มีอยู่ในหน่วยความจำ
> - Owner เดิมจะไม่สูญเสีย Ownership ไป
> - วิธีนี้จะทำให้เข้าถึงข้อมูลได้อย่างมีประสิทธิภาพสูงสุด เนื่องจากไม่ต้อง Copy ข้อมูลทั้งก้อนจาก Heap
>   ไปยังตัวแปรใหม่ และข้อมูลยังคงมีเพียงที่เดียวเหมือนเดิม

##### Example 4:

```rust, editable
fn main() {
    let s1 = String::from("Hello");

    // ยืมข้อมูลจาก s1 มาชั่วคราวโดยไม่ต้องย้าย Ownership
    let s2 = &s1;

    // สามารถยืมข้อมูลจาก s1 ได้หลายตัว
    let s3 = &s1;

    // หรือสามารถ Copy Memory Address จาก s2 ไปยัง s4 ตรงๆแบบนี้ก็ได้
    let s4 = s2;

    print_string(s2);

    // จะเกิด error เนื่องจาก fn print_string ต้องการ argument ที่เป็นตัวแปรอ้างอิงเท่านั้น
    print_string(s1);
}

// หากต้องการใช้ argument ที่เป็นตัวแปรอ้างอิง จำเป็นต้องประกาศ type ให้เป็น `&` ด้วย
fn print_string(s: &String) {
    println!("{}", s);
}
```

#### 5. Immutable borrows allow many references at a time

> [!TIP]
>
> - สามารถ immutable borrow ได้หลายตัวเท่านั้น

##### Example 5:

```rust, editable
fn main() {
    let s1 = String::from("Hello");

    // สามารถ immutable borrow s1 ได้หลายตัว
    let s2 = &s1;
    let s3 = &s1;
    let s4 = &s1;

    println!("s2: {}, s3: {}, s4: {}", s2, s3, s4);
}
```

#### 6. Mutable borrow allows only one reference

> [!TIP]
>
> - หากต้องการใช้ mutable borrow ต้องประกาศตัวแปร Owner เป็น `mut` ก่อนเท่านั้น

##### Example 6:

```rust, editable
fn main() {
    // จำเป็นต้องประกาศ Owner เป็น `mut` ก่อน ถึงจะสามารถ mutable borrow ได้
    let mut s1 = String::from("Hello");

    // สามารถ mutable borrow s1 ได้หนึ่งตัวเท่านั้น
    let s2 = &mut s1;
    // error: cannot borrow `s1` as mutable more than once at a time
    // let s3 = &mut s1;

    println!("s2: {}", s2);
}
```

#### 7. Mutable and immutable references cannot coexist

> [!TIP]
>
> - เพื่อป้องกันความ inconsistent state ระหว่าง Reader และ Writer,
>   ทำให้รับประกันได้ว่าข้อมูลจะไม่ถูกแก้ไขระหว่างการที่มีการ Read อยู่

##### Example 7:

```rust, editable
fn main() {
    let mut s1 = String::from("Hello");

    let s2 = &s1;
    let s3 = &mut s1;
    // error: cannot borrow `s1` as immutable because it is also borrowed as mutable

    println!("s2: {}, s3: {}", s2, s3);
}
```

#### 8. References must be valid (lifetime)

> [!TIP]
>
> - Rust มีความสามารถกำหนด Lifetime ของตัวแปรอ้างอิงได้อย่างชัดเจน
> - Lifetime คือ ช่วงเวลาที่ตัวแปรอ้างอิงยังคงมีอยู่ในหน่วยความจำ
> - Lifetime จะช่วยให้ Compiler ตรวจสอบว่าตัวแปรอ้างอิงยังคงมีอยู่ในหน่วยความจำหรือไม่
>   ตั้งแต่ตอน Compile Time
> - Lifetime ช่วยป้องกัน dangling references
>   โดยรับประกันว่าข้อมูลที่ถูกอ้างอิงจะมีอายุยาวนานกว่าหรือเท่ากับตัวแปรที่อ้างอิงถึงมัน
> - Rust มักจะสามารถอนุมาน lifetime ได้โดยอัตโนมัติ แต่บางครั้งเราอาจต้องระบุ lifetime
>   ด้วยตนเองโดยใช้ syntax เช่น `'a`, `'b`
> - การใช้ lifetime
>   ที่ถูกต้องช่วยให้เราสามารถสร้างและใช้งานโครงสร้างข้อมูลที่ซับซ้อนซึ่งมีการอ้างอิงถึงกันและกันได้อย่างปลอดภัย

##### Example 8.1: Struct with lifetime

```rust, editable
#[derive(Debug)]
struct Person<'a> { // 'a คือ lifetime ของตัวแปรอ้างอิง
    name: String,
    age: u8,
    friend: Option<&'a Person<'a>>,
}

fn main() {
    let person;                         // --------------+---> 'person
                                        //               |  
    {                                   //               |
        let friend = Person {           // ---+---> 'freind 
            name: String::from("Bob"),  //    |          |
            age: 30,                    //    |          |
            friend: None,               //    |          |
        };                              //    |          |
                                        //    |          |
        person = Person {               //    |          |
            name: String::from("Bob"),  //    |          |
            age: 30,                    //    |          |
            friend: Some(&friend),      //    | - lifetime 'a ของ struct Person<'a> จะถูก assign ด้วย 'friend
        };                              //    |          |
                                        //    |          |
    }                                   // ---+          |
                                        //               |  
    // error: `friend` does not live long enough         |
    println!("{:?}", person);           //               |
}                                       // --------------+
```

##### Example 8.2: Function with lifetime

```rust, editable
fn longest<'a, 'b>(x: &'a str, y: &'b str) -> &'a str {
    if x.len() > y.len() {
        x
    } else {
        y
    }
}

fn main() {
    let s1 = "long string is long".to_string();    // ------------+---> 's1, lifetime 'a ของ fn longest ถูก assign เป็น 's1
    let result;                                    //             +---> 'result
    {                                              //             |
        let s2 = "xyz".to_string();                // --+---> 's2 | lifetime 'a ถูกย้ายมายัง 's2 (เนื่องจากมีอายุสั้นกว่า)
        result = longest(&s1, &s2);                //   |         |
    }                                              // --+         |
                                                   //             |
    // จะเกิด error result does not live long enough               |         
    println!("The longest string is '{result}'");  //             |
}                                                  // ------------+
```

## Exercises

### 1. Move Semantics

แก้ Compiler Error ต่อไปนี้ โดยให้แก้ไขได้เฉพาะการเพิ่มหรือการลบตัวแปรอ้างอิง (ตัวอักษร `&`)
เท่านั้น

```rust, editable
// TODO: Fix the compiler errors without changing anything except adding or
// removing references (the character `&`).

// Shouldn't take ownership
fn get_char(data: String) -> char {
    data.chars().last().unwrap()
}

// Should take ownership
fn string_uppercase(mut data: &String) {
    data = data.to_uppercase();

    println!("{data}");
}

fn main() {
    let data = "Rust is great!".to_string();

    get_char(data);

    string_uppercase(&data);
}
```

### 2. Mutable Borrowing

เขียนฟังก์ชัน `update_and_print` ที่รับ mutable reference ของ Vec<String>
แล้วทำการเพิ่มข้อความ " (updated)" ต่อท้ายทุกสตริงใน Vec นั้น จากนั้นพิมพ์ทุกสตริงออกมา

```rust, editable
fn update_and_print() {
    // Your code here
}

fn main() {
    let my_vec: Vec<String> = vec![
        String::from("Apple"),
        String::from("Banana"),
        String::from("Cherry")
    ];
    update_and_print(&my_vec);

    // This should print:
    // Apple (updated)
    // Banana (updated)
    // Cherry (updated)
}
```

### 3. Lifetime Management

จงแก้ Compiler Error ต่อไปนี้ โดยห้ามแก้ไขส่วนของฟังก์ชัน `longest`

```rust, editable
// Don't change this function.
fn longest<'a>(x: &'a str, y: &'a str) -> &'a str {
    if x.len() > y.len() {
        x
    } else {
        y
    }
}

fn main() {
    // TODO: Fix the compiler error by moving one line.

    let string1 = String::from("long string is long");
    let result;
    {
        let string2 = String::from("xyz");
        result = longest(&string1, &string2);
    }
    println!("The longest string is '{result}'");
}
```
