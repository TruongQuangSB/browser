# Native componten for Chromium

## Project structure

The native component is split into serveral subcomponents

- `chromium`: Contains generated Rust bindings for CEF
- `chromium_subp`: Implements the chromium subprocess executable 
- `chromium_swt`: CEF implementation and communication with `chromium_subp`
- `chromium_jni`: Dynamic library to provide JNI bindings for `chromium_swt`
- `chromium_jni_utils`: Utilities for `chromium_jni`
- `chromium_jni_macro`: Macros for `chromium_jni` and JNI<->Rust conversion

## chromium_jni

When modifying `chromium_jni` care must be taken to keep changes in sync with the associated Java code in `org.eclipse.set.browser.cef.lib`. 

## Build native library and subprocess executable

### Prerequisites

- Rust (https://www.rust-lang.org/en-US/install.html)

- Download CEF binary distrubution (minimal is enough for release builds), unzip it and rename to [gitroot]/cef

### Build

#### Build native library and subprocess helper executable

- cd [gitroot]/native
- `cargo build --release`

### Bindings

CEF bindings for Rust are autogenerated. Types to generate are configured in chromium/build.rs (whitelisted and blacklisted types) 
and cef_*.h header files in chromium/ folder.
This uses https://rust-lang.github.io/rust-bindgen/introduction.html. See there for specific details about configuration options and how to tune the generation.

#### Generate Rust CEF Bindings

This additionally requires an installed LLVM clang for bindgen. 

- cd [gitroot]/native
- `cargo build --features gen --release`