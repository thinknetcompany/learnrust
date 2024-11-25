# บทที่ 4: การร่ายคาถา

<div>
  <audio controls loop autoplay>
    <source src="../audio/chap4.mp3" type="audio/mpeg">
    เบราว์เซอร์ของคุณไม่รองรับการเล่นเสียง
  </audio>
</div>

> "ในที่สุดข้าก็เข้าใจ... การจะเอาชนะจอมมารไม่ใช่แค่การมีพลังมากกว่า แต่เป็นการรู้จักผสมผสานพลังและความรู้เข้าด้วยกัน เหมือนการร่ายคาถาที่ต้องร้อยเรียงท่วงท่าและคำพูดให้สมบูรณ์แบบ"

![chap4-open](../images/story/chap4-open.png)

## คาถาพื้นฐาน (Basic Functions)
****
> [!NOTE]
> ฟังก์ชันถูกประกาศโดยใช้คีย์เวิร์ด `fn` โดย argument ต้องระบุชนิดข้อมูล (type annotation) เช่นเดียวกับตัวแปร และถ้าฟังก์ชันต้องการส่งค่ากลับ จะต้องระบุชนิดข้อมูลที่ส่งกลับหลังเครื่องหมายลูกศร `->`
>
> นิพจน์สุดท้ายในฟังก์ชันจะถูกใช้เป็นค่าที่ส่งกลับ หรือสามารถใช้คำสั่ง `return` เพื่อส่งค่ากลับก่อนจบฟังก์ชันได้ แม้จะอยู่ภายในลูปหรือเงื่อนไข `if` ก็ตาม
>

```rust, editable
fn summon_power(base_power: u32, multiplier: u32) -> u32 {
    let total_power = base_power * multiplier;
    println!("Collecting power {} units", total_power);
    total_power
}

fn main() {
    let power = summon_power(100, 5);
    println!("Total power: {}", power);
}
```

### คาถาป้องกัน

```rust, editable
fn create_barrier(power: u32, name: &str) -> String {
    // ตัวอย่าง return แบบเต็ม
    if power > 100 {
        return format!("Create barrier {}: Protection power {}", name, power);
    }
    
    // ตัวอย่าง return แบบย่อ (ละคำสั่ง return)
    format!("Create barrier {}: Protection power {}", name, power)
}

fn main() {
    println!("{}", create_barrier(150, "Magic"));
    println!("{}", create_barrier(50, "Element"));
}
```

## คาถาผสาน (Function Composition)

```rust, editable
fn enchant_weapon(weapon_name: &str) -> String {
    format!("{}", weapon_name)
}

fn add_elemental_power(weapon: String, element: &str) -> String {
    format!("{} of {}", weapon, element)
}

fn main() {
    let basic_weapon = enchant_weapon("Dragon");
    let powered_weapon = add_elemental_power(basic_weapon, "Fire");
    println!("{} created!", powered_weapon);
}
```

## แบบฝึกหัดการร่ายคาถา:
---
### บททดสอบการรวมพลัง
ให้เติม syntax ที่ถูกต้องลงในช่องว่าง
```rust, editable
// เติมฟังก์ชันให้สมบูรณ์
fn combine_elements(____: &str, ____: &str) -> String {
    format!("Combine {} and {} elements: {}", ____, ____, ____)
}

fn main() {
    println!("{}", combine_elements("Fire", "Wind"));
    // Should show: "Combine Fire and Wind elements: FireWind"
}
```

### บททดสอบการสร้างคาถาต่อเนื่อง
ให้เติม syntax ที่ถูกต้องลงในช่องว่าง
```rust, editable
fn charge_power(____: u32) -> u32 {
    ____
}

fn release_spell(____: u32, ____: &str) -> String {
    format!("Release power {} units on {}", ____, ___)
}

fn main() {
    let charged_power = charge_power(50);
    let spell_result = release_spell(charged_power, "Demon lord");
    println!("{}", spell_result);
    // Should show: "Release power 150 units on Demon lord!"
}
```

### บททดสอบการสร้างคาถาสุดท้าย
ให้เติม syntax ที่ถูกต้องลงในช่องว่าง
```rust, editable
enum MagicResult {
    Success(String),
    Failure(String),
    Backfire(u32),
}

fn cast_ultimate_spell(
    power: u32,
    skill: u32,
    mana: u32
) -> Result<MagicResult, String> {
    if ____ < 100 {
        return Err("Not enough mana...".to_string());
    }

    ____ (power, skill) {
        (____, ____) if ____ > 1000 && s > 100 => Ok(MagicResult::Success(format!("Release power {} units on {}", power, skill))),
        (p, _) if p > 500 => Ok(MagicResult::Success(format!("Release power {} units on {}", power, skill))),
        _ => Err(format!("Not enough mana..."))
    }
}

fn main() {
    match cast_ultimate_spell(1200, 120, 150) {
        Ok(MagicResult::Success(msg)) => println!("Success! {}", msg),
        Ok(MagicResult::Failure(msg)) => println!("Failed: {}", msg),
        Ok(MagicResult::Backfire(damage)) => println!("Spell backfired! Take {} damage", damage),
        Err(e) => println!("Cannot cast spell: {}", e),
    }
}
```

> "ทุกคาถาที่ข้าเรียนรู้ ทุกพลังที่ข้าได้รับ ล้วนนำข้าเข้าใกล้จุดหมายมากขึ้น... แต่การเดินทางยังไม่จบ ข้าต้องเรียนรู้ที่จะรับมือกับความล้มเหลวที่อาจเกิดขึ้น..."

![chap4-close](../images/story/chap4-close.png)

ติดตามการผจญภัยต่อใน [บทที่ 5: การเผชิญหน้าครั้งสุดท้าย](./error_handling.md) ที่จะเผยถึงวิธีการรับมือกับอุปสรรคและความผิดพลาดที่อาจเกิดขึ้น...
