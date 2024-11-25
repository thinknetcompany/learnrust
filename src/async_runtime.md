# Asynchronous Runtime

> [!NOTE]
>
> - โดยปกติแล้ว การทำงานของ Rust จะเป็น Synchronous หมายความว่า
>   ทุกอย่างจะต้องทำงานอย่างต่อเนื่องจากการทำงานของตัวมันเอง
> - แต่ถ้าหากต้องการทำงานแบบ Parallel หรือ Asynchronous, Rust
>   เองก็มีการสนับสนุนในการทำงานแบบนั้นได้ โดยการใช้งาน Thread หรือ Task หรือ Future
>   (คล้ายๆ Promise ใน NodeJS) ด้วยเช่นกัน
> - แต่ก็มีทางเลือกอีกทางที่ง่ายกว่านั้น ก็คือการใช้งาน Async Runtime ผ่าน Crate ที่ชื่อว่า `Tokio`
> - `Tokio` คือ Crate ที่สร้างขึ้นมาเพื่อสนับสนุนการทำงานแบบ Asynchronous และ Parallel ใน
>   Rust โดยทำหน้าที่จัดการการทำงานของ Thread หรือ Task หรือ Future ให้ง่ายขึ้น ผ่าน Syntax
>   `Async/Await` ที่เราคุ้นเคยกันมาจาก NodeJS
> - Web framework หรือ Database ส่วนใหญ่ของ Rust จะทำงานแบบ Asynchronous อยู่บน
>   `Tokio Runtime` เช่น `Axum`, `Actix`, `Rocket`, `Warp`

> [!TIP]
>
> - `Async/Await` ของ `Tokio Rust` จะใช้งานคล้ายกับ `Async/Await` ของ NodeJS
>   แต่เบื้องหลังการทำงานจะแตกต่างกัน
>   - สำหรับ NodeJS จะใช้งานผ่าน `Event Loop` ที่ทำงานอยู่ในภายใน Single Thread
>   - สำหรับ Rust จะใช้งานผ่าน `Thread` หรือ `Task` ที่ทำงานอยู่ในภายใน `Thread Pool`
>     ซึ่งจะทำงานอยู่บน Core ของ CPU ที่มีทั้งหมด ทำให้ Rust สามารถทำงานแบบ Parallel
>     ได้โดยสมบูรณ์

## How Tokio Runtime Work

> [!TIP]
>
> - **Thread Pool**: `Tokio` ใช้ thread pool ในการจัดการ task ต่างๆ โดยจะมีการสร้าง
>   thread ขึ้นมาเป็นจำนวนหนึ่งตามจำนวน core ของ CPU ที่มีอยู่
> - **Task Scheduling**: `Tokio` มี task scheduler ที่ทำหน้าที่จัดสรร task ต่างๆ ให้กับ
>   thread pool เพื่อให้การทำงานเป็นไปอย่างมีประสิทธิภาพ
> - **Async/Await**: การใช้งาน `async` และ `await` ใน `Tokio`
>   จะช่วยให้การเขียนโค้ดแบบ asynchronous ง่ายขึ้น โดยไม่ต้องจัดการกับ thread โดยตรง
> - **Event Loop**: `Tokio` มี event loop ที่ทำหน้าที่ตรวจสอบ event ต่างๆ และจัดการกับ
>   task ที่พร้อมจะทำงาน
> - **Resource Management**: `Tokio` จัดการ resource ต่างๆ เช่น network connection,
>   file I/O, และ timer ให้เป็นไปอย่างมีประสิทธิภาพ
> - **Thread Safety**: `Tokio` จัดการ thread ให้เป็นไปอย่างปลอดภัยในการเข้าถึง resource
>   ต่างๆ โดยใช้ `Send` และ `Sync` trait ของ Rust เพื่อให้แน่ใจว่า data ที่ถูกแชร์ระหว่าง
>   thread นั้นปลอดภัย

## How to use Tokio Runtime

### Add Dependency

```toml
[dependencies]
tokio = { version = "1.0", features = ["macro", "rt-multi-thread"] }
```

or

```sh
cargo add tokio -F rt-multi-thread,macro
```

### Change Main Function to `#[tokio::main]`

```rust
#[tokio::main]
async fn main() {
    println!("Hello, world!");
}
```

### Use `Async/Await` Function

> [!TIP]
>
> - `async fn` คือ Function ที่ทำงานแบบ Asynchronous (ไม่ block การทำงานของ Thread)
> - `async fn` จะต้องถูกเรียกใช้ผ่าน `.await` เสมอ
> - code ที่อยู่บรรทัดถัดจาก `.await` จะต้องรอการทำงานของ `async fn`
>   ให้เสร็จสิ้นก่อนจึงจะทำงานต่อได้
> - เหมาะกับงานประเภท `I/O-bound` ที่มักจะใช้เวลาส่วนใหญ่ในการรอการตอบสนองจากระบบภายนอก
>   (เช่น การอ่าน/เขียนไฟล์, การเชื่อมต่อเครือข่าย) `async fn` ช่วยให้สามารถปล่อยทรัพยากร
>   (เช่น thread) ให้ทำงานอื่นได้ในระหว่างที่รอ I/O
> - จะต้องประกาศ `fn main` เป็น `async fn` ด้วย `#[tokio::main]` ก่อนเท่านั้น

```rust
// จำเป็นต้องมีการประกาศ `#[tokio::main]` เพื่อทำให้ application นี้รองรับการทำงานแบบ Asynchronous
#[tokio::main]
async fn main() {
    // required `.await` to run async function
    do_something().await;
}

// Define `async` keyword in front of function
async fn do_something() {
    // TODO: Implement
}
```

### Use `Spawn` Task to Run Background Process

> [!TIP]
>
> - `tokio::spawn` จะสร้าง task ใหม่ที่ทำงานอยู่เบื้องหลังของโปรแกรม
>   และจะเริ่มทำงานทันที่ถูกสร้าง task
> - `tokio::spawn` จะแตกต่างจาก `async fn` ตรงที่ `tokio::spawn`
>   ไม่จำเป็นต้องรอการทำงานของ task ให้เสร็จสิ้นก่อนที่จะทำงาน code ในบรรทัดถัดไปได้
> - เหมาะกับงานประเภท `CPU-bound` ที่สามารถทำงานพร้อมกันหลาย process ได้
> - สามารถรอ task ให้ทำงานเสร็จสิ้นก่อนที่จะทำงานต่อได้ผ่าน `.await` ได้เช่นกัน
> - จะต้องประกาศ `fn main` เป็น `async fn` ด้วย `#[tokio::main]` ก่อนเท่านั้น

```rust
use tokio::time::{sleep, Duration};

#[tokio::main]
async fn main() {
    println!("เริ่มโปรแกรม");

    // สร้าง task เพื่อให้ทำงานอยู่เบื้องหลังของโปรแกรม
    let task = tokio::spawn(async {
        for i in 1..=5 {
            sleep(Duration::from_secs(1)).await;
            println!("Task ทำงาน: ครั้งที่ {}", i);
        }
        println!("Task เสร็จสมบูรณ์!");
    });

    // รอสักครู่เพื่อให้เห็นผลของ task
    println!("รอ task ทำงาน");
    sleep(Duration::from_secs(3)).await;
    println!("หมดเวลา");

    // รอ task ให้ทำงานเสร็จสิ้น
    // task.await.unwrap();
    println!("โปรแกรมหลักจบการทำงาน");
}
```
