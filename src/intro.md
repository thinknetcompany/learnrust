# Introduction

![Rust Logo](./images/Rust-logo.jpg)

## What is Rust?

Rust เป็นภาษา Programming สมัยใหม่ ถูกสร้างมาเพื่อแก้ไขปัญหาของภาษา C,C++ เช่น
ความปลอดภัยเกี่ยวกับ Memory โดยไม่จำเป็นต้องใช้ Garbage Collector
แต่ยังคงประสิทธิภาพสูงใกล้เคียงกับภาษา C,C++ อยู่ และสามารถทำงานแบบ Concurrent
(Multi-threaded) ได้อย่างปลอดภัยและมีประสิทธิภาพ

ปัจจุบันเริ่มมีความนิยมในการนำ Rust มาใช้ในงานด้าน Web Development อย่างแพร่หลายมากขึ้น โดยมี
Development Tools สมัยใหม่ๆที่เริ่มได้รับความนิยม เช่น

- [Turbo Repo & Turbo Pack](https://turbo.build/)
- [SWC - Speedy Web Compiler](https://swc.rs/)
- [Rspack - The fast Rust-based web bundler](https://rspack.dev/)
- [Biomejs - A Rust-based linter & formatter](https://biomejs.dev/)
- [Deno](https://deno.com/)
- [SurrealDB](https://surrealdb.com/)
- [Meilisearch](https://www.meilisearch.com/)
- [Cloudflare](https://www.cloudflare.com/)
- ฯลฯ

### Maintainer

Rust ได้จัดตั้งเป็นองค์กรอิสระที่มีชื่อว่า
[Rust Foundation](https://foundation.rust-lang.org/) เพื่อดูแลและพัฒนาภาษา Rust
อย่างเป็นระบบอย่างสม่ำเสมอ โดยได้รับการสนับสนุนจากบริษัทใหญ่ๆมากมาย เช่น Microsoft, AWS,
Google, Meta, Huawei และ Mozilla เป็นต้น

![Rust Foundation](./images/Rust-foundation.jpg)

## Pros / Cons

### Pros

- ประสิทธิภาพสูง (ใกล้เคียงกับภาษา C,C++)
- Memory Safety (Google และ Microsoft กล่าวว่า มากกว่า 70% ของปัญหา Software
  มาจากปัญหา Memory และปัญหา Software hacks จะหมดไปหากเราเปลี่ยนไปใช้ภาษา Rust -
  [Ref.](https://www.danielfullstack.com/article/70-of-all-software-hacks-will-be-gone-if-we-move-to-rust))
- ไม่มี Garbage Collector
- เป็นภาษา Strongly Typed
- ออกแบบมาให้สามารถทำงานแบบ Concurrent (Multi-threaded) ได้อย่างปลอดภัย
  (Thread-safe)
- สามารถ Compile เป็น Binary code ที่สามารถ Run บน OS Layer ได้โดยตรง (no runtime
  overhead)
- มี Compiler ที่ฉลาดมากๆ สามารถตรวจสอบความถูกต้องได้ตั้งแต่ Compile Time
  รวมถึงให้คำแนะนำสำหรับ Potential Error Code ที่อาจเกิดขึ้นได้
- ทำให้เราสามารถ Maintenance ในอนาคตได้อย่างมั่นใจมากขึ้น (จาก Strongly Typed +
  Compiler + Strict Rules ของ Rust)
- มีระบบ Inline documentation ที่ดีเยี่ยม
- มี Features ของภาษาสมัยใหม่ๆ มากมาย เช่น
  - Cargo - Package Manager: คล้ายกับ `npm` ใน Node.js สำหรับจัดการ Package และ
    Dependency ของ Project
  - Async/Await: คล้ายกับ `async/await` ใน JavaScript สำหรับการจัดการงานแบบ
    Asynchronous (แต่ใน Rust จะเป็นการทำงานแบบ Multi-threaded)
  - Macros
  - Generics
  - Traits
  - Pattern Matching
  - Smart Pointers
  - อื่นๆ

### Cons

- มี Learning curve ค่อนข้างสูงในตอนเริ่มต้น
  - เนื่องจากมี Features สมัยใหม่ๆค่อนข้างมาก เราจะเจอสิ่งใหม่ๆที่แปลกตามากขึ้น
    จึงทำให้ต้องใช้เวลาศึกษามากขึ้น
  - เนื่องจากเป็นภาษา Safety สูง จึงจำเป็นต้องมี Rules ที่เคร่งครัดมากขึ้น
- ใช้เวลาในการ Compile ที่ค่อนข้างนาน
  (เนื่องจากมีกระบวนการตรวจสอบความถูกต้องทางด้านความปลอดภัยที่เข้มข้น และการ optimization
  ที่ละเอียด)
- ความยืดหยุ่นในการเขียน Code น้อยลง เนื่องจากเป็นภาษา Strongly Typed และมี Rules
  ที่ต้องตรวจสอบความปลอดภัย
- ใช้งาน JSON ค่อนข้างยุ่งยาก เนื่องจากต้องมีการแปลงข้อมูลไปมาระหว่าง JSON และ struct ในภาษา
  Rust

## Other Showcases

- [Compare - How Much Memory Do You Need to Run 1 Million Concurrent Tasks?](https://pkolaczk.github.io/memory-consumption-of-async/)
- [Why Discord is switching from Go to Rust](https://discord.com/blog/why-discord-is-switching-from-go-to-rust)
- [Microsoft Start Using Rust for Windows Kernel](https://www.blognone.com/node/133635)
- [Google Migrate Android Kernel from C++ to Rust](https://www.blognone.com/node/131693)
- [Google Plans to Migrate Android Platform Tools & Libraries from Go, C++ to Rust](https://www.techtalkthai.com/google-reveals-rust-developers-are-2x-more-productive-than-c-teams/)
- [TRACTOR - US Government to Convert All C Code to Rust](https://akshatjoshi.hashnode.dev/us-government-to-convert-all-c-code-to-rust)
- [US Government Endorse Rust Over C,C++](https://medium.com/@rahul.rocket711/us-government-endorse-rust-372138f6c502)
- [Apple Recruit Rust Developers for Migrate CloudKit from C to Rust](https://www.blognone.com/node/118394)
- [Rust for Linux Kernel](https://rust-for-linux.com/)
- [Samsung TizenOS](https://www.blognone.com/node/136116)
- [BlueOS](https://droidsans.com/vivo-launch-its-self-development-os-blueos/)
