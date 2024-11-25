# Installation

## Install Rust

1. Run คำสั่งต่อไปนี้ (ใช้ได้เฉพาะบน Unix-like OS เช่น Linux, macOS, WSL2)

   ```bash
   curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
   ```

   หรือเข้าไปที่ [Rust Installation](https://www.rust-lang.org/tools/install)

2. ตรวจสอบว่า Rust ถูกติดตั้งหรือไม่โดยใช้คำสั่งต่อไปนี้

   ```bash
   rustc --version
   ```

3. ตรวจสอบว่า Cargo ถูกติดตั้งหรือไม่โดยใช้คำสั่งต่อไปนี้

   ```bash
   cargo --version
   ```

4. ติดตั้ง Extension ดังต่อไปนี้ เพื่อใช้ในการพัฒนาโปรแกรมด้วย Rust
   1. VSCode Extensions
      1. [Rust Analyzer](https://marketplace.visualstudio.com/items?itemName=rust-lang.rust-analyzer) -
         เพื่อทำให้ VSCode รู้จักภาษา Rust และสามารถตรวจสอบความถูกต้องของ Code ได้
         รวมถึงเป็นตัวช่วย auto-complete, linter, formatter ไปในตัวด้วย
      2. [Dependi](https://marketplace.visualstudio.com/items?itemName=fill-labs.dependi) -
         เอาไว้ตรวจสอบ Version และเข้าถึง Doc ของ Dependency ที่เราใช้ใน Project
      3. [Even Better TOML](https://marketplace.visualstudio.com/items?itemName=tamasfe.even-better-toml) -
         เพื่อทำให้ VSCode รู้จักรูปแบบของ TOML และสามารถใช้จัด Format และ auto-complete
         ได้
      4. [Error Lens](https://marketplace.visualstudio.com/items?itemName=usernamehw.errorlens)
         (Optional) - ใช้แสดง Error หรือ Warning บนบรรทัดที่เกิดขึ้นได้อย่างชัดเจนขึ้น
   2. ติดตั้ง Cargo Extension ด้วยคำสั่ง `cargo install <extension-name>` ดังต่อไปนี้
      1. [cargo-watch](https://github.com/watchexec/cargo-watch) - เอาไว้ใช้ Run
         project โดยสามารถ Watch การเปลี่ยนแปลงของไฟล์ได้ (ทำงานคล้ายกับ `nodemon` ใน
         Node.js)
      2. [cargo-run-script](https://github.com/JoshMcguigan/cargo-run-script) -
         เอาไว้ใช้ Run script ที่อยู่ใน `Cargo.toml` ได้
      3. [cargo-nextest](https://github.com/nextest-rs/cargo-nextest) -
         Alternative test runner ของ Rust ซึ่งมีประสิทธิภาพสูงกว่า `cargo test`
         และอ่านผลลัพธ์ได้ง่ายขึ้น

## Update Rust

1. Run คำสั่งต่อไปนี้
   ```bash
   rustup update <:channel>
   ```

   - `channel` คือช่องทางการอัปเดต ซึ่งสามารถเลือกได้เพียง 3 ช่องทาง คือ `stable`, `beta`
     หรือ `nightly` เท่านั้น

2. ตรวจสอบ Version ของ Rust หลังจากอัปเดตแล้ว

   ```bash
   rustc --version
   ```

## Uninstall Rust

1. Run คำสั่งต่อไปนี้

   ```bash
   rustup self uninstall
   sudo rm -rf $HOME/.cargo
   sudo rm -rf $HOME/.rustup
   ```

2. ตรวจสอบว่า Rust ถูกถอดออกแล้วหรือไม่โดยใช้คำสั่งต่อไปนี้

   ```bash
   rustc --version
   ```

3. ตรวจสอบว่า Cargo ถูกถอดออกแล้วหรือไม่โดยใช้คำสั่งต่อไปนี้

   ```bash
   cargo --version
   ```
