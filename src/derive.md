- [Derive Macro](#derive-macro)
  - [Debug](#debug)
  - [Example Derive Debug](#example-derive-debug)
  - [Clone](#clone)
      - [Example Derive Clone](#example-derive-clone)
  - [Copy](#copy)
      - [Example Derive Copy](#example-derive-copy)
  - [PartialEq](#partialeq)
  - [Eq](#eq)
      - [Example Derive PartialEq and Eq](#example-derive-partialeq-and-eq)
  - [PartialOrd](#partialord)
      - [Example Derive PartialOrd](#example-derive-partialord)
  - [Ord](#ord)
      - [Example Derive PartialOrd and Ord](#example-derive-partialord-and-ord)
  - [Default](#default)
      - [Example Derive Default](#example-derive-default)
  - [Exercise](#exercise)

# Derive Macro

> [!NOTE]
>
> - `Derive Macro` คือ `procedural macro` ชนิดหนึ่ง
> - `Derive Macro` คือ shorthand impl Trait ให้กับตัวแปรชนิดต่างๆ เช่น struct หรือ enum
>   โดยที่เราไม่ต้องเขียน impl Trait ด้วยตัวเอง
> - `Derive Macro` ที่ได้ใช้บ่อยๆ มีดังนี้
>   - `Debug`
>   - `PartialEq`
>   - `Eq`
>   - `PartialOrd`
>   - `Ord`
>   - `Clone`
>   - `Copy`
>   - `Default`

> [!WARNING]
>
> การ Derive ความสามารถให้กับ struct หรือ enum เกินความจำเป็นจะทำให้มีผลกระทบต่อ
> Performance เนื่องจากมีการ Allocate/Deallocate Heap Memory ที่สูงตามมา

## Debug

> [!NOTE]
> ใช้สำหรับเพิ่มความสามารถในการ debug ค่าในตัวแปร เช่น struct หรือ enum ผ่าน
> `println!("{:?}", value)` หรือ `dbg!(value)` โดยที่เราไม่ต้องเขียน impl Debug
> ด้วยตัวเอง

## Example Derive Debug

```rust, editable
#[derive(Debug)]
struct Rectangle {
    width: u32,
    height: u32,
}

fn main() {
    let rect1 = Rectangle {
        width: 30,
        height: 50,
    };

    println!("rect1 is {:?}", &rect1);
    // หรือ
    dbg!(&rect1);
}
```

## Clone

> [!NOTE]
> ใช้สำหรับเพิ่มความสามารถในการ Clone ค่าของ struct หรือ enum ไปสร้างตัวแปรใหม่ ผ่าน
> method `.clone()`

> [!TIP]
> การ Clone จะทำให้ตัวแปรที่ถูก Clone มีค่าเหมือนตัวแปรต้น แต่เป็นตัวแปรใหม่ ที่มี Ownership
> เป็นของตัวเอง

#### Example Derive Clone

```rust, editable
#[derive(Clone)]
struct Rectangle {
    width: u32,
    height: u32,
}

fn main() {
    let rect1 = Rectangle {
        width: 30,
        height: 50,
    };

    let rect2 = rect1.clone();

    // react1 จะยังสามารถใช้งานได้ เนื่องจากไม่มีการย้าย Ownership
    println!("rect1: {:?}", rect1);
    println!("rect2: {:?}", rect2);
}
```

## Copy

> [!NOTE]
> ใช้สำหรับเพิ่มความสามารถในการ Clone ค่าของ struct หรือ enum ไปยังตัวแปรอื่น
> **<u>โดยอัตโนมัติ</u>** โดยไม่ใช้ `Move Semantics` (เหมือน Primitive Types)

> [!WARNING]
>
> - ต้องใช้คู่กับ Derive `Clone` พร้อมกันเท่านั้น
> - ใช้เมื่อจำเป็นเท่านั้น เนื่องจากมี Cost ในการ Allocate/Deallocate Heap Memory ที่สูงตามมา
>   ทำให้มีผลกระทบต่อ Performance

#### Example Derive Copy

```rust, editable
#[derive(Clone, Copy)]
struct Rectangle {
    width: u32,
    height: u32,
}

fn main() {
    let rect1 = Rectangle {
        width: 30,
        height: 50,
    };

    // rect1 จะถูก Copy ไปยัง rect2 โดยอัตโนมัติ โดยไม่ต้องใช้ method `.clone()`
    // ซึ่งทำให้ rect1 ยังสามารถใช้งานได้ เนื่องจากไม่มีการย้าย Ownership
    let rect2 = rect1;

    // rect1 จะยังสามารถใช้งานได้ เนื่องจากไม่มีการย้าย Ownership
    println!("rect1: {:?}", rect1);
    println!("rect2: {:?}", rect2);
}
```

## PartialEq

> [!NOTE]
>
> - ใช้สำหรับเพิ่มความสามารถในการเปรียบเทียบความเท่ากัน `(== หรือ !=)` ของ struct หรือ
>   enum
> - อนุญาตให้มีค่าที่ไม่สามารถเปรียบเทียบกันได้ (เช่น NaN ใน floating-point number)

## Eq

> [!NOTE]
>
> - `Eq` เป็น trait ที่สืบทอดมาจาก `PartialEq`
> - `Eq` รับประกันว่าความสัมพันธ์ความเท่ากันเป็นความสัมพันธ์สมมูล (equivalence relation)
>   ทางคณิตศาสตร์ ดังนี้
>   - สะท้อน (reflexive): สำหรับทุกค่า `a`, `a == a` จะต้องเป็นจริง
>   - สมมาตร (symmetric): สำหรับทุกค่า `a` และ `b`, ถ้า `a == b`, `b == a`
>     จะต้องเป็นจริง
>   - ถ่ายทอด (transitive): สำหรับทุกค่า `a`, `b`, และ `c`, ถ้า `a == b` และ
>     `b == c`, `a == c` จะต้องเป็นจริง

> [!WARNING]
>
> - ต้องใช้คู่กับ Derive `PartialEq` พร้อมกันเท่านั้น
> - ไม่สามารถใช้กับ type ที่ไม่สามารถเปรียบเทียบกันได้ (เช่น NaN ใน floating-point number)

#### Example Derive PartialEq and Eq

```rust, editable
// สามารถใช้ Eq ได้เนื่องจาก i32 มี equivalence relation
#[derive(PartialEq, Eq)]
struct RectangleInt {
    width: i32,
    height: i32,
}

// ไม่สามารถใช้ Eq ได้ เนื่องจาก f64 มีโอกาสเป็น NaN ซึ่งไม่สามารถเปรียบเทียบกันได้
#[derive(PartialEq)]
struct RectangleFloat {
    width: f64,
    height: f64,
}

fn main() {
    let rect_int1 = RectangleInt {
        width: 30,
        height: 50,
    };
    let rect_int2 = RectangleInt {
        width: 30,
        height: 50,
    };

    let rect_float1 = RectangleFloat {
        width: f64::NAN,
        height: 50.0,
    };
    let rect_float2 = RectangleFloat {
        width: f64::NAN,
        height: 50.0,
    };

    // true
    println!("rect_int1 == rect_int2: {}", rect_int1 == rect_int2);
    // false เนื่องจาก NaN ไม่สามารถเปรียบเทียบกันได้
    println!("rect_float1 == rect_float2: {}", rect_float1 == rect_float2);
}
```

## PartialOrd

> [!NOTE]
>
> - ใช้สำหรับเพิ่มความสามารถในการเปรียบเทียบแบบมีลำดับ (Ordering) ของ struct หรือ enum
>   ด้วย `<`, `>`, `<=`, `>=`
> - จะเปรียบเทียบค่า `<`, `>`, `<=`, `>=` ตามลำดับ property ของ struct หรือ enum
>   และจะ return ค่า boolean จาก property แรกที่มีค่า `<`, `>`, `<=`, `>=`
>   กับคู่ที่เปรียบเทียบ

> [!WARNING]
>
> - `PartialOrd` ต้องการ `PartialEq` เป็นพื้นฐาน

#### Example Derive PartialOrd

```rust, editable
#[derive(PartialEq, PartialOrd)]
struct Rectangle {
    width: u32,
    height: u32,
}

fn main() {
    let rect1 = Rectangle {
        width: 30,
        height: 50,
    };

    let rect2 = Rectangle {
        width: 30,
        height: 50,
    };

    println!("rect1 < rect2: {}", rect1 < rect2);
}
```

## Ord

> [!NOTE]
>
> - `Ord` เป็น trait ที่สืบทอดมาจาก `PartialOrd` และ `Eq`
> - รับประกันว่าทุกคู่ของค่าสามารถเปรียบเทียบกันได้และมีลำดับที่ชัดเจนเท่านั้น (Total ordering)

> [!WARNING]
>
> - ต้องใช้คู่กับ Derive `PartialOrd`, `PartialEq`, และ `Eq`
> - ไม่สามารถใช้กับ type ที่ไม่สามารถเปรียบเทียบกันได้ (เช่น NaN ใน floating-point number)

#### Example Derive PartialOrd and Ord

```rust, editable
// สามารถใช้ Ord ได้เนื่องจาก i32 มี total ordering
#[derive(PartialEq, PartialOrd, Eq, Ord)]
struct RectangleInt {
    width: i32,
    height: i32,
}

// ไม่สามารถใช้ Eq ได้ เนื่องจาก f64 มีโอกาสเป็น NaN ซึ่งไม่สามารถเปรียบเทียบกันได้
#[derive(PartialEq, PartialOrd)]
struct RectangleFloat {
    width: f64,
    height: f64,
}

fn main() {
    let rect_int1 = RectangleInt {
        width: 30,
        height: 50,
    };
    let rect_int2 = RectangleInt {
        width: 30,
        height: 50,
    };

    let rect_float1 = RectangleFloat {
        width: f64::NAN,
        height: 50.0,
    };
    let rect_float2 = RectangleFloat {
        width: f64::NAN,
        height: 50.0,
    };

    // true
    println!("rect_int1 < rect_int2: {}", rect_int1 < rect_int2);
    // false เนื่องจาก NaN ไม่สามารถเปรียบเทียบกันได้
    println!("rect_float1 < rect_float2: {}", rect_float1 < rect_float2);
}
```

## Default

> [!NOTE]
> ใช้สำหรับเพิ่มความสามารถในการสร้าง struct หรือ enum
> ตัวใหม่ให้มีค่าเป็นค่าเริ่มต้นของตัวแปรชนิดต่างๆ ผ่าน method `::default()`

#### Example Derive Default

```rust, editable
#[derive(Default)]
struct Rectangle {
    width: u32,
    height: u32,
}

// หากเป็น enum จำเป็นต้องกำหนดค่า default เองด้วย `#[default]` attribute
#[derive(Debug, Default, PartialEq)]
enum Test {
    A,
    #[default]
    // กำหนดให้ B เป็นค่า default ของ enum Test
    B,
    C,
}

fn main() {
    let rect1 = Rectangle::default();
    let test_enum = Test::default();
    
    assert_eq!(rect1.width, 0);
    assert_eq!(rect1.height, 0);
    assert_eq!(test_enum, Test::B);
}
```

## Exercise

1. ทำให้ struct `User` สามารถ Debug ได้

```rust, editable
enum UserType {
    Admin,
    User,
}

struct User {
    name: String,
    age: u8,
    user_type: UserType,
}


fn main() {
    let user = User {
        name: "John Doe".to_string(),
        age: 20,
        user_type: UserType::Admin,
    };

    println!("user: {:?}", user);
}
```

2. ทำให้ struct `User` สามารถเปรียบเทียบความเท่ากันได้ (`==` และ `!=`)

```rust, editable
enum UserType {
    Admin,
    User,
}

struct User {
    name: String,
    age: u8,
    user_type: UserType,
}

fn main() {
    let user1 = User {
        name: "John Doe".to_string(),
        age: 20,
        user_type: UserType::Admin,
    };

    let user2 = User {
        name: "Jane Doe".to_string(),
        age: 20,
        user_type: UserType::User,
    };

    println!("user1 == user2: {}", user1 == user2);
}
```

3. ทำให้ struct `User` สามารถ Clone ได้

```rust, editable
enum UserType {
    Admin,
    User,
}

struct User {
    name: String,
    age: u8,
    user_type: UserType,
}

fn main() {
    let user1 = User {
        name: "John Doe".to_string(),
        age: 20,
        user_type: UserType::Admin,
    };

    let user2 = user1.clone();
    
    println!("user1: {:?}", user1);
    println!("user2: {:?}", user2);
}
```
