- [JSON](#json)
  - [Add serde and serde\_json to Cargo.toml](#add-serde-and-serde_json-to-cargotoml)
  - [Convert String to JSON Directly](#convert-string-to-json-directly)
  - [Serialize](#serialize)
      - [Example Serialize](#example-serialize)
  - [Deserialize](#deserialize)
      - [Example Deserialize](#example-deserialize)

# JSON

> [!NOTE]
>
> - Rust ไม่สามารถทำงานกับ JSON ได้โดยตรงเหมือนกับ NodeJS
> - ต้องใช้ 3rd-party crate เข้ามาช่วย ได้แก่ `serde` และ `serde_json`
> - `serde_json' จะสามารถช่วยแปลงค่า String หรือ struct ของ Rust ให้กลายเป็น JSON ได้
> - JSON ใน Rust จะมี type เป็น `serde_json::Value`
> - มี Derive Macro จาก `serde` ที่ช่วยในการทำงานระหว่าง struct กับ JSON 2 ตัวดังนี้
>   - `Serialize` ช่วยในการแปลง struct ให้กลายเป็น JSON
>   - `Deserialize` ช่วยในการแปลง JSON ให้กลายเป็น struct

## Add serde and serde_json to Cargo.toml

```toml
[dependencies]
serde = { version = "1.0", features = ["derive"] }
serde_json = "1.0"
```

or

```sh
cargo add serde serde_json -F serde/derive
```

## Convert String to JSON Directly

> [!TIP]
>
> - `serde_json` มี macro_fn ชื่อ `json!` ที่ช่วยในการแปลง String หรือตัวแปรอื่นๆ
>   ให้กลายเป็น JSON ได้แบบง่ายๆ ดังตัวอย่างด้านล่าง

> [!WARNING]
>
> - แต่การทำเช่นนี้ จะทำให้ตัวแปรที่ได้สูญเสียความสามารถของการตรวจสอบ Type ของข้อมูลและทำให้
>   Intellisense ของ IDE สูญหายไป

```rust, editable
use serde_json::{json, Value};

fn main() {
    let json: Value = json!({
        "name": "John Doe",
        "age": 30
    });

    println!("{}", json);
}
```

## Serialize

> [!TIP]
>
> - ใช้ในการแปลง struct ให้กลายเป็น JSON

#### Example Serialize

```rust, editable
use serde::{Serialize, json};

#[derive(Serialize)]
struct User {
    name: String,
    age: u8,
}

fn main() {
    let user = User {
        name: "John Doe".to_string(),
        age: 30,
    };

    let json = serde_json::to_value(&user).unwrap();

    println!("{}", json);
}
```

## Deserialize

> [!TIP]
>
> - ใช้ในการแปลง JSON ให้กลับเป็น struct

#### Example Deserialize

```rust, editable
use serde::{Deserialize, json};

#[derive(Debug, Deserialize)]
struct User {
    name: String,
    age: u8,
}

fn main() {
    let json: Value = serde_json::json!({
        "name": "John Doe",
        "age": 30
    });

    let user: User = serde_json::from_value(json).unwrap();

    println!("{:?}", user);
}
```
