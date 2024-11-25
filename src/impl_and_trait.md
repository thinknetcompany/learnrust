- [Impl \& Trait](#impl--trait)
  - [Impl](#impl)
      - [Example impl](#example-impl)
  - [Trait](#trait)
      - [Example trait](#example-trait)
    - [Inheritance Trait](#inheritance-trait)
      - [Example inheritance trait](#example-inheritance-trait)
    - [Genric Trait Bound](#genric-trait-bound)
      - [Example Generic Trait Bound](#example-generic-trait-bound)

# Impl & Trait

## Impl

> [!NOTE]
> Impl หรือ Implementation คือการกำหนดความสามารถเพิ่มเติมให้กับตัวแปรชนิดต่างๆ เช่น struct
> หรือ enum โดยการกำหนด method เพิ่มเติมให้กับตัวแปรนั้นๆ

#### Example impl

```rust, editable
struct Rectangle {
    width: u32,
    height: u32,
}

impl Rectangle {
    // สร้าง Instance ใหม่ของ struct นี้
    fn new(width: u32, height: u32) -> Self {
        Self { width, height }
    }

    // เราสามารถกำหนดความสามารถเพิ่มเติมให้กับ struct ได้
    // ในที่นี้เรากำหนดความสามารถในการหาพื้นที่ของรูปสี่เหลี่ยมผ่าน method area
    // &self คือการอ้างอิงตัวเอง
    fn area(&self) -> u32 {
        self.width * self.height
    }
}

fn main() {
    let rect = Rectangle::new(30, 50);
    println!("Area of rectangle is {}", rect.area());
}
```

## Trait

> [!NOTE]
> Trait คือ กลุ่มของคุณลักษณะ (methods) ที่กำหนดไว้ล่วงหน้า สามารถนำ impl ให้กับ struct หรือ
> enum เพื่อให้ struct หรือ enum เหล่านั้นมีความสามารถตามที่กำหนดใน trait เหล่านั้นได้

#### Example trait

```rust, editable
// กำหนด trait คุณลักษณะที่ Animal มีร่วมกัน
trait Animal {
    fn eat(&self, food: &str) -> ();
    fn can_make_sound(&self) -> bool;

    // เราสามารถกำหนดค่า Default ให้กับ method ได้
    fn can_sleep(&self) -> bool {
        true
    }
}

struct Dog;
struct Cat;

// นำ Trait ไปใช้กับ struct
// เราจำเป็นต้อง implement ทุก method ที่ไม่มีค่า Default ใน trait นั้นๆ
impl Animal for Dog {
    fn eat(&self, food: &str) -> () {
        println!("Dog eat {}", food);
    }

    fn can_make_sound(&self) -> bool {
        true
    }
}

impl Animal for Cat {
    fn eat(&self, food: &str) -> () {
        println!("Cat eat {}", food);
    }
    
    fn can_make_sound(&self) -> bool {
        false
    }

    // สามารถ Override ค่า Default ได้
    fn can_sleep(&self) -> bool {
        false
    }
}

fn main() {
    let dog = Dog;
    let cat = Cat;

    dog.eat("Bone");
    cat.eat("Fish");

    println!("Dog can make sound: {}", dog.can_make_sound());
    println!("Cat can make sound: {}", cat.can_make_sound());

    println!("Can dog sleep: {}", dog.can_sleep());
    println!("Can cat sleep: {}", cat.can_sleep());
}
```

### Inheritance Trait

> [!NOTE]
> เราสามารถสืบทอดคุณลักษณะของ Trait อื่นๆ ให้กับ Trait ที่เราสร้างได้โดย Syntax นี้:\
> `trait ChildTrait: ParentTrait_1 + ParentTrait_2 + ...`

> [!TIP]
> การ inheritance `trait ChildTrait: ParentTrait_1 + ParentTrait_2 + ...`
> นี้ไม่ได้หมายความว่า `ChildTrait` จะมีคุณลักษณะของ `ParentTrait` ทั้งหมด แต่หมายความว่า
> ก่อนจะสามารถใช้ `ChildTrait` ได้ จะต้องมีคุณลักษณะของ `ParentTrait` ทั้งหมดก่อนเสมอ

#### Example inheritance trait

```rust, editable
trait Animal {
    fn eat(&self, food: &str) -> ();
    fn can_make_sound(&self) -> bool;
}
trait SomeOtherTrait {
    fn some_other_method(&self) -> ();
}

trait DogTrait: Animal + SomeOtherTrait {
    fn bark(&self) -> ();
}

struct Dog;

impl DogTrait for Dog {
    fn eat(&self, food: &str) -> () {
        println!("Dog eat {}", food);
    }

    fn can_make_sound(&self) -> bool {
        true
    }

    fn bark(&self) -> () {
        println!("Dog bark");
    }
}

fn main() {
    let dog = Dog;
    dog.eat("Bone");
    dog.bark();
}
```

### Genric Trait Bound

> [!NOTE]
> เราสามารถใช้ประโยชน์ของ Trait ในการกำหนดคุณลักษณะของ Function ได้โดยการใช้
> `Generic Trait Bound` เพื่อระบุว่า `argument` หรือ `return type` ต้องมีคุณลักษณะอะไรบ้าง

> [!TIP]
> การใช้ `Generic Trait Bound` นี้จะทำให้เรามีความยืดหยุ่นในการเขียนโปรแกรมได้มากขึ้น
> โดยเราสามารถเขียนได้ 3 วิธี ดังนี้:
>
> 1. `fn function_name<T: TraitName>(arg: T) -> T`
> 2. `fn function_name<T>(arg: T) -> T where T: TraitName`
> 3. `fn function_name(arg: impl TraitName) -> impl TraitName`

#### Example Generic Trait Bound

```rust, editable
trait Animal {
    fn eat(&self, food: &str) -> ();
}

struct Dog;
struct Cat;

impl Animal for Dog {
    fn eat(&self, food: &str) -> () {
        println!("Dog eat {}", food);
    }
}

impl Animal for Cat {
    fn eat(&self, food: &str) -> () {
        println!("Cat eat {}", food);
    }
}

fn main() {
    let dog = get_animal("Dog");
    let cat = get_animal("Cat");

    feed_animal(&dog);
    feed_animal(&cat);
}

fn get_animal(type: &str) -> impl Animal {
fn get_animal(type: &str) -> impl Animal {
    if type == "Dog" {
        Dog
    } else {
        Cat
    }
}

fn feed_animal(animal: &impl Animal) {
    animal.eat("Food");
}
```
