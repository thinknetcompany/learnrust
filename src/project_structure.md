# Project Structure

```tree
project_name/
├── Cargo.toml ------- เป็นไฟล์ที่ระบุข้อมูลของโปรเจ็คทั้งหมด (เหมือน package.json)
├── Cargo.lock ------- ใช้เก็บ cache ของ dependencies (เหมือน package-lock.json)
├── src -------------- เขียน code ใน src/ เท่านั้น, สิ่งที่อยู่ใน src/ ทั้งก้อนจะถูกเรียกว่า crate
│   ├── main.rs ------ เป็น entrypoint ของโปรเจ็ค ในกรณีที่เป็น binary crate
│   ├── lib.rs ------- เป็น entrypoint ของโปรเจ็ค ในกรณีที่เป็น library crate
│   └── utils/ ------- ทุก file หรือ folder ที่อยู่ใน src จะถูกเรียกว่า module
│       ├── mod.rs --- ทุกครั้งที่สร้าง folder ใหม่ จะต้องมีไฟล์ mod.rs เสมอ (คล้ายๆ index.js)
│       └── ...
├── target/ ---------- เก็บไฟล์ที่ถูก build แล้ว
└── tests/ ----------- folder สำหรับเขียน integration test
```

## Crate Type

> [!NOTE]
> ใน Rust มีสองประเภทของ crate คือ `binary crate` และ `library crate`
>
> - `binary crate` จะสามารถ compile เป็น executable ได้และสามารถ run ได้โดยตรง เช่น
>   application, server, script เป็นต้น โดยจะมีไฟล์ `main.rs` เป็น entrypoint
> - `library crate` จะไม่สามารถ run ได้โดยตรง แต่จะถูกใช้เป็น dependencies ในโปรเจ็คอื่นๆ
>   โดยจะมีไฟล์ `lib.rs` เป็น entrypoint

## Cargo.toml

> [!NOTE]
> ทำหน้าที่ระบุรายละเอียดของโปรเจ็คทั้งหมด (เหมือน package.json)

```toml
[package] # ระบุรายละเอียดของโปรเจ็ค
    authors      = ["THiNKNET"]
    edition      = "2021" # ระบุ Rust edition
	license      = "Copyright © 2018 THiNKNET Co., Ltd. All Rights Reserved."
	name         = "project_name" # ต้องเป็น snake_case
	readme       = "README.md"
	repository   = "https://gitlab.thinknet.co.th/techforum/rust/<project_name>"
	rust-version = "1.80" # minimum rust version
	version      = "0.1.0" # version ของโปรเจ็ค

# Find more dependencies at https://crates.io/
[dependencies] # ระบุ dependencies ของโปรเจ็ค
    serde      = { version = "1.0", features = ["derive"] } # สามารถตั้งค่าการ import ได้หลายอย่าง
    serde_json = "1.0" # หรือจะระบุแค่ version ก็ได้

[dev-dependencies] # ระบุ dependencies ของโปรเจ็ค เฉพาะในการพัฒนา (เหมือน devDependencies ใน package.json)
    pretty_assertions = "1"
```

## How to Create new Module in Project

> [!TIP]
> ทุกไฟล์หรือ folder ที่อยู่ใน src/ จะถูกเรียกว่า module

### File

#### Example New File Module

สมมติว่าต้องการสร้าง module ใหม่ที่ชื่อว่า `my_func`

1. สร้างไฟล์ใหม่ที่ชื่อว่า `my_func.rs` ใน `src/`

   ```tree
   src/
   ├── main.rs
   └── my_func.rs
   ```

2. เพิ่ม function หรือ struct ต่างๆ ในไฟล์ `my_func.rs`

   ```rust
   // src/my_func.rs

   // ต้องใส่ pub (ย่อจาก public) นำหน้าของ function หรือ struct หากต้องการให้สามารถเรียกใช้จากภายนอกได้
   pub fn my_func() {
       println!("Hello, world!");
   }

   pub struct MyStruct {
       // ต้องใส่ pub นำหน้าของ field ใน struct ด้วยเช่นกัน หากต้องการให้สามารถเรียกใช้จากภายนอกได้
       pub name: String,
   }
   ```

3. ประกาศ `mod my_func`(ชื่อไฟล์) ในไฟล์ `main.rs` หรือ `lib.rs` เพื่อให้ project รู้จัก
   module นี้

   ```rust
   // src/main.rs

   mod my_func; // ประกาศว่า project นี้มี module ชื่อว่า `my_func` อยู่

   use my_func::MyStruct; // ใช้ use (เหมือน import ในภาษาอื่นๆ) เพื่อเรียกใช้ module นี้

   fn main() {
       let my_struct = MyStruct { name: "John".to_string() };
       println!("{}", my_struct.name);
   }
   ```

### Folder

สมมติว่าต้องการสร้าง module ใหม่ที่ชื่อว่า `my_utils/` และภายใน folder นี้มีไฟล์หลายๆ ไฟล์

1. สร้างไฟล์ใหม่ที่ชื่อว่า `my_utils/` ใน `src/`

   ```tree
   src/
   ├── main.rs
   └── my_utils/
       ├── mod.rs ------------ หากสร้าง folder จะต้องมีไฟล์นี้เสมอ (คล้ายๆ index.js แต่ใน Rust บังคับให้ต้องมี)
       ├── my_func.rs
       └── my_struct.rs
   ```

2. เพิ่ม function หรือ struct ต่างๆ ลงในไฟล์ลูกภายใน `my_utils/`
   เหมือนกันกับการสร้างไฟล์แบบด้านบน
3. ภายในไฟล์ `mod.rs` ของ `my_utils/` ต้องประกาศว่ามี module `my_func` และ
   `my_struct` อยู่ (คล้ายๆกับ main.rs แต่ mod.rs มีหน้าที่ควบคุม module ย่อยของตัวเอง)

   ```rust
   // src/my_utils/mod.rs

   // pub mod หากต้องการให้ module ย่อยนี้สามารถเรียกใช้จากภายนอกได้
   pub mod my_func;
   // หากไม่ใส่ pub จะทำให้ module ย่อยนี้เป็น private ซึ่งจะสามารถเรียกใช้ได้ภายใน folder นี้เท่านั้น
   mod my_struct;
   ```

4. ประกาศ `mod my_utils`(ชื่อ folder) ในไฟล์ `main.rs` หรือ `lib.rs` เพื่อให้ project
   รู้จัก module นี้

   ```rust
   // src/main.rs

   mod my_utils; // ประกาศว่า project นี้มี module ชื่อว่า `my_utils` อยู่
   ```
