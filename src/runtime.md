r[runtime]
# The Rust runtime

This section documents features that define some aspects of the Rust runtime.

r[runtime.panic_handler]
## The `panic_handler` attribute

r[runtime.panic_handler.allowed-positions]
The *`panic_handler` attribute* can only be applied to a function with signature
`fn(&PanicInfo) -> !`.

r[runtime.panic_handler.intro]
The function marked with this [attribute] defines the behavior of panics.

r[runtime.panic_handler.panic-info]
The [`PanicInfo`] struct contains information about the location of the panic.

r[runtime.panic_handler.unique]
There must be a single `panic_handler` function in the dependency graph of a binary, dylib or cdylib crate.

Below is shown a `panic_handler` function that logs the panic message and then halts the
thread.

<!-- ignore: test infrastructure can't handle no_std -->
```rust,ignore
#![no_std]

use core::fmt::{self, Write};
use core::panic::PanicInfo;

struct Sink {
    // ..
#    _0: (),
}
#
# impl Sink {
#     fn new() -> Sink { Sink { _0: () }}
# }
#
# impl fmt::Write for Sink {
#     fn write_str(&mut self, _: &str) -> fmt::Result { Ok(()) }
# }

#[panic_handler]
fn panic(info: &PanicInfo) -> ! {
    let mut sink = Sink::new();

    // logs "panicked at '$reason', src/main.rs:27:4" to some `sink`
    let _ = writeln!(sink, "{}", info);

    loop {}
}
```

r[runtime.panic_handler.std]
### Standard behavior

The standard library provides an implementation of `panic_handler` that
defaults to unwinding the stack but that can be [changed to abort the
process][abort]. The standard library's panic behavior can be modified at
runtime with the [set_hook] function.

r[runtime.global_allocator]
## The `global_allocator` attribute

The *`global_allocator` attribute* is used on a [static item] implementing the
[`GlobalAlloc`] trait to set the global allocator.

r[runtime.windows_subsystem]
## The `windows_subsystem` attribute

r[runtime.windows_subsystem.intro]
The *`windows_subsystem` attribute* may be applied at the crate level to set
the [subsystem] when linking on a Windows target.

r[runtime.windows_subsystem.syntax]
It uses the [_MetaNameValueStr_] syntax to specify the subsystem with a value of either
`console` or `windows`.

r[runtime.windows_subsystem.ignored]
This attribute is ignored on non-Windows targets, and for non-`bin` [crate types].

r[runtime.windows_subsystem.console]
The "console" subsystem is the default. If a console process is run from an
existing console then it will be attached to that console, otherwise a new
console window will be created.

r[runtime.windows_subsystem.windows]
The "windows" subsystem is commonly used by GUI applications that do not want to
display a console window on startup. It will run detached from any existing console.

```rust
#![windows_subsystem = "windows"]
```

[_MetaNameValueStr_]: attributes.md#meta-item-attribute-syntax
[`GlobalAlloc`]: alloc::alloc::GlobalAlloc
[`PanicInfo`]: core::panic::PanicInfo
[abort]: ../book/ch09-01-unrecoverable-errors-with-panic.html
[attribute]: attributes.md
[crate types]: linkage.md
[set_hook]: std::panic::set_hook
[static item]: items/static-items.md
[subsystem]: https://msdn.microsoft.com/en-us/library/fcc1zstk.aspx
