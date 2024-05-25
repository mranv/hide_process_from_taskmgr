# Hide Process From Task Manager

## Overview

This project provides functions and hooks for manipulating processes and memory on Windows systems using the Windows API. The code includes functionalities to read and write process memory, parse import and export tables, and hook system functions.

## Dependencies

- [winapi](https://docs.rs/winapi/)
- [ntapi](https://docs.rs/ntapi/)
- [windows](https://docs.rs/windows/)

Ensure you have these dependencies in your `Cargo.toml` file.

## Functions

### 1. `ReadStringFromMemory`

Reads a string from the memory of a specified process.

```rust
pub fn ReadStringFromMemory(prochandle: *mut c_void, base: *const c_void) -> String
```

### 2. `messageboxclone`

Custom implementation of `MessageBoxA` that hooks into the function and alters its behavior based on the content of `lptext`.

```rust
pub unsafe extern "C" fn messageboxclone(hwnd: *mut HWND__, lptext: *const i8, lptitle: *const i8, boxtype: u32) -> i32
```

### 3. `test2`

Hooks into the `NtQuerySystemInformation` function, intercepting calls to it and modifying its behavior.

```rust
pub unsafe extern "C" fn test2(sysinfo: u32, baseaddress: *mut c_void, infolength: u32, outinfolength: *mut u32) -> i32
```

### 4. `FillStructureFromArray`

Fills a structure from a byte array.

```rust
pub fn FillStructureFromArray<T, U>(base: &mut T, arr: &[U]) -> usize
```

### 5. `FillStructureFromMemory`

Fills a structure from a memory location.

```rust
pub fn FillStructureFromMemory<T>(dest: &mut T, src: *const c_void, prochandle: *mut c_void) -> usize
```

### 6. `ParseExports64`

Parses the export table of a 64-bit process and returns a map of function names to addresses.

```rust
pub fn ParseExports64(prochandle: *mut c_void, baseaddress: *mut c_void) -> HashMap<String, i32>
```

### 7. `ParseImports64`

Parses the import table of a 64-bit process and returns a map of DLL names to functions and their addresses.

```rust
pub fn ParseImports64(prochandle: *mut c_void, baseaddress: *mut c_void) -> HashMap<String, Vec<HashMap<String, HashMap<String, i64>>>>
```

### 8. `DllMain`

Entry point for the DLL, sets up hooks for specific functions.

```rust
pub unsafe extern "stdcall" fn DllMain(handle: HINSTANCE, reason: u32, reserved: *mut c_void) -> u32
```

## Usage

### Building the Project

To build the project, run:

```sh
cargo build
```

### Running the Project

To run the project, you will need to load the compiled DLL into a target process. Ensure you have the necessary permissions to manipulate process memory and hook system functions.

### Example

To hook into `NtQuerySystemInformation` and modify its behavior:

1. Load the DLL into the target process.
2. The `DllMain` function will set up the hook.
3. Calls to `NtQuerySystemInformation` will be intercepted by `test2`.

## Notes

- This project is intended for educational purposes and should be used responsibly.
- Ensure you have the necessary permissions and understanding of the implications of manipulating process memory.

## License

This project is licensed under the MIT License.

## Acknowledgments

- [winapi](https://docs.rs/winapi/) for Windows API bindings.
- [ntapi](https://docs.rs/ntapi/) for NT API bindings.
- [windows](https://docs.rs/windows/) for Windows API bindings.

Feel free to contribute to this project by submitting issues or pull requests.
