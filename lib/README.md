# lib

Put the C library here: `sqlite.lib` to link with, and `sqlite.dll` when it is a DLL.
Then bind its functions in `src/sqlite.fels`:

```fels
#extern("sqlite", "sqlite_init")
pub init :: fn(flags: u32) -> i32;
```

and in `build.fels` uncomment the lines for the DLL. `fels bindgen sqlite.h --lib sqlite`
writes the bindings of a whole header for you.
