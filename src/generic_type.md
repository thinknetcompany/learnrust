# Generic Type

> [!NOTE]
> Generic Type ใน Rust ช่วยให้เราสามารถเขียนโค้ดที่ยืดหยุ่นและนำกลับมาใช้ใหม่ได้
> โดยไม่ต้องระบุชนิดข้อมูลที่แน่นอนล่วงหน้า ซึ่งทำให้โค้ดสามารถทำงานกับชนิดข้อมูลหลายๆ ชนิดได้

> [!WARNING]
> การใช้ Generic Type กับ Function จำเป็นต้องระบุ Trait Bound ตามที่ Function ต้องการ

## Use Generic Type with Struct

```rust, editable
struct Point<T> {
    x: T,
    y: T,
}

impl<T> Point<T> {
    fn new(x: T, y: T) -> Self {
        Self { x, y }
    }
}

fn main() {
    // T จะกลายเป็นชนิดข้อมูลที่กำหนดในที่นี้คือ i32 และ f32
    let integer_point = Point { x: 1, y: 2 };
    let float_point = Point::new(1.0, 2.0);
}
```

## Use Generic Type with Function

```rust, editable
struct Rectangle {
    width: u16,
    height: u16,
}

trait HasArea {
    fn area(&self) -> u16;
}

impl Rectangle {
    fn new(width: u16, height: u16) -> Self {
        Self { width, height }
    }
}

impl HasArea for Rectangle {
    fn area(&self) -> u16 {
        self.width * self.height
    }
}

fn print_area<T: HasArea>(shape: T) {
    println!("Area: {}", shape.area());
}

fn main() {
    let rect = Rectangle::new(10, 20);
    print_area(rect);
}
```
