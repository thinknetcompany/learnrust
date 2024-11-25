# Display Trait

> [!NOTE]
>
> - `Display trait` ใช้ในการเพิ่มความสามารถให้กับ struct หรือ enum
>   ให้สามารถแสดงผลผ่านการเรียกใช้ฟังก์ชัน `format!` หรือ `println!` ได้
>   รวมถึงได้ความสามารถในการแปลงข้อมูลเป็น String ผ่าน method `to_string()` ได้
> - เนื่องจาก `Display trait` ไม่มี Derive Macro ให้ใช้งาน จึงจำเป็นต้อง impl ด้วยตัวเอง

#### Example iml Display trait

```rust, editable
use std::fmt::{Display, Formatter, Result};

struct Test {
    name: String,
    age: u8,
}

impl Display for Test {
    fn fmt(&self, f: &mut Formatter<'_>) -> Result {
        write!(f, "Test {{ name: {}, age: {} }}", self.name, self.age)
    }
}

fn main() {
    let test = Test {
        name: "John".to_string(),
        age: 20,
    };

    let test_string: String = test.to_string();

    println!("{}", test);
}
```
