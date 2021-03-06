# GameBoy debugger

Compares the results of file `expected.txt` and `got.txt`.

### Create the file `expected.txt`

- Clone [this](https://github.com/benkonz/gameboy_emulator) repository (an existing Game Boy emulator)
- Insert the following code in file `gameboy_core/src/cpu/mod.rs` at line `142`

```rust
let mut flags = 0;
        if self.registers.f.contains(Flag::ZERO) {
            flags += 0b1000;
        }
        if self.registers.f.contains(Flag::NEGATIVE) {
            flags += 0b100;
        }
        if self.registers.f.contains(Flag::HALF_CARRY) {
            flags += 0b10;
        }
        if self.registers.f.contains(Flag::FULL_CARRY) {
            flags += 0b1;
        }

        println!(
            "PC:{:#x},SP:{:#x},OP:{:#x},OP+1:{:#x},A:{:#x},BC:{:#x},DE:{:#x},HL:{:#x},F:{:#06b},",
            self.registers.pc,
            self.registers.sp,
            opcode,
            memory.read_byte(self.registers.pc),
            self.registers.a,
            ((self.registers.b as u16) << 8) + (self.registers.c as u16),
            ((self.registers.d as u16) << 8) + (self.registers.e as u16),
            ((self.registers.h as u16) << 8) + (self.registers.l as u16),
            flags,
        );

```

- Execute the following command in the `gameboy_emulator` root folder

```sh
    cargo run --package gameboy_opengl -- [path to test rom] > ../gamyboy-debugger/expected.txt
```

### Create the file `got.txt`

Print the state of the emulator you are trying to debug before each CPU instructions is executed in the same format as above

### Run the check script

If you have not already, install [NodeJs](https://nodejs.org/en/)

Run the following command

```sh
    node check.js
```

The script may fail if the putout files are too large, in that case you may run the `check.go` file instead with the following command (requires installing [go](https://golang.org/doc/install))

```sh
    go run ./check.go
```
