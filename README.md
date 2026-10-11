# sqlite

[Fels](https://github.com/fels-lang/fels) bindings to [SQLite 3](https://sqlite.org).

**State:** created for Strata (milestone M2). The code is still the C binding template; the bindings come with M2.

## Planned

- The SQLite DLL (`sqlite3.dll`) and its import library in `lib/`, provided to the users of the
  package by `build.fels`.
- The raw bindings in `src/sqlite.fels` (`sqlite3_open_v2`, `sqlite3_prepare_v2`,
  `sqlite3_bind_*`, `sqlite3_step`, `sqlite3_column_*`, `sqlite3_finalize`, `sqlite3_close`,
  `sqlite3_exec`, `sqlite3_errmsg`), written by `fels bindgen sqlite3.h --lib sqlite3`.
- A thin Fels layer over them: open a database, prepare a statement, bind values, step through
  rows, transactions, errors as Fels errors.

## Use it

```
fels add sqlite github.com/<you>/sqlite
```

```fels
#import sqlite;
```

## Work on it

```
fels test        run the tests (they call SQLite)
fels build run   build and run examples/demo.fels
```
