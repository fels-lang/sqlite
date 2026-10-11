# lib

SQLite 3.54.0 for 64-bit Windows, built from the official amalgamation:

| File | What it is |
| --- | --- |
| `sqlite3.dll` | the library; `build.fels` copies it next to the programs that use the package |
| `sqlite3.lib` | its import library, to link with |
| `sqlite3.h` | the header of this version, for reference and `fels bindgen` |

The DLL needs only `kernel32` and the Universal C Runtime, which every Windows 10 and 11 has.

## How it was built

`sqlite-amalgamation-3540000.zip` from https://www.sqlite.org/download.html, checked against
the SHA3-256 that page gives (`7b670a62fdfbd672b75fef004cb703c8a3e87d3a5cc7d675b4a08337004a2d93`),
then:

```
zig cc -target x86_64-windows-gnu -O2 -shared -DSQLITE_DQS=0 -DSQLITE_THREADSAFE=1 -DSQLITE_DEFAULT_FOREIGN_KEYS=1 -DSQLITE_OMIT_DEPRECATED -DSQLITE_DEFAULT_WAL_SYNCHRONOUS=1 "-DSQLITE_API=__declspec(dllexport)" sqlite3.c -o sqlite3.dll
```

with Zig 0.16.0. The options: no double-quoted string literals, safe to use from several
threads, foreign keys checked by default, no deprecated functions, and in WAL mode a normal
sync.

SQLite is in the public domain.
