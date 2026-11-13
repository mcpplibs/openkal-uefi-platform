# openkal-uefi-platform

The platform package (mcpp SPEC-013) of UEFI applications for x86_64 above
[openkal-uefi][impl]: the row `x86_64-uefi` as data, its compile and link
conventions, and QEMU's q35 board with the edk2 firmware as its runner.

```toml
[target.x86_64-uefi]
platform = "openkal-uefi-platform@0.1.0"
```

```sh
mcpp build --target x86_64-uefi     # PE32+, subsystem EFI application
mcpp run   --target x86_64-uefi     # booted by dirun under QEMU; the status comes back
```

| Part | Where |
|---|---|
| the row: `x86_64-unknown-uefi`, PE, `.efi`, execution `uefi` | `[[platform.define]]` |
| freestanding, no red zone, UCS-2 `wchar_t`, entry `efi_main` | `[platform.build]` |
| the block-memory functions a compiler calls (no C library) | `runtime/mem.S` |
| the board's exit: the status to QEMU's debug-exit port, only under QEMU | `board/qemu-exit.S` |
| the runner: dirun boots the application as `EFI/BOOT/BOOTX64.EFI` | `[platform.runner]` |

The engine knows no UEFI: everything above is this package's (design P15).

[impl]: https://github.com/mcpplibs/openkal-uefi
