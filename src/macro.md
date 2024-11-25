- [Macro](#macro)
  - [Declarative Macros](#declarative-macros)
  - [Procedural Macros](#procedural-macros)
      - [Example Procedural Macros](#example-procedural-macros)

# Macro

> [!NOTE]
> ใน Rust มี macro อยู่ 2 ประเภทคือ
>
> 1. `macro_fn!()` หรือ `declarative macros` - มีลักษณะและการใช้งานเหมือนกับ function
>    ปกติ แต่ทำงานต่างกันที่ `macro_fn!()` จะทำการสร้างโค้ดตามที่เรากำหนดไว้ขณะ compile
>    time ในขณะที่ function จะทำงานที่ runtime
> 2. `#[macro_export]` หรือ `procedural macros` หรือ `attribute macros` -
>    จะใช้สำหรับกำหนดคุณลักษณะให้กับระบบการทำงานของ Rust เพื่อสร้างโค้ดตามที่เรากำหนดไว้ขณะ
>    compile time

## Declarative Macros

> [!NOTE]
> ข้อดีเมื่อเปรียบเทียบกับ function
>
> 1. ไม่มี runtime overhead: code จะถูก inline expand ในขั้นตอน compile-time
> 2. มีความยืดหยุ่นสูง: สามารถสร้างโค้ดที่ซับซ้อนได้โดยใช้ pattern matching
>
> ข้อจำกัด:
>
> 1. ยากต่อการเขียนและดีบัก: มี Syntax เฉพาะที่ค่อนข้างซับซ้อน ทำให้โค้ดอ่านยากขึ้น:
> 2. ข้อความแสดงข้อผิดพลาดมักจะไม่ชัดเจนเท่ากับฟังก์ชัน
> 3. ไม่สามารถใช้ในทุกบริบท: มีข้อจำกัดในการใช้งานบางสถานการณ์

## Procedural Macros

> [!TIP]
> โดยส่วนมากแล้ว เรามักจะได้ใช้ `procedural macros` ในการกำหนดคุณลักษณะให้กับ struct,
> enum อย่างง่ายๆ (ทางลัด) ผ่าน `#[derive]`

#### Example Procedural Macros

```rust
#[derive(Debug, Clone)]
struct User {
    name: String,
    age: u8,
}

// #[derive(Debug, Clone)] จะทำงานเหมือนกับการ manually impl Debug, Clone เองตามด้านล่าง
// impl Debug for User {
//     fn fmt(&self, f: &mut Formatter) -> Result {
//         write!(f, "User {{ name: {}, age: {} }}", self.name, self.age)
//     }
// }

// impl Clone for User {
//     fn clone(&self) -> Self {
//         User {
//             name: self.name.clone(),
//             age: self.age,
//         }
//     }
// }
```
