# sqlite

[Fels](https://github.com/fels-lang/fels) bindings to [SQLite 3](https://sqlite.org), with the
SQLite library (3.54.0, 64-bit Windows) included.

```fels
#import sqlite;

db := sqlite::open("notes.db") catch err { return 1; };
db.exec("CREATE TABLE IF NOT EXISTS notes (id INTEGER PRIMARY KEY, text TEXT NOT NULL);") catch err { return 2; };

ins := db.prepare("INSERT INTO notes (text) VALUES (?1);") catch err { return 3; };
ins.bind_text(1, "hello") catch err { return 4; };
done := ins.step() ?? false;
ins.finish();

q := db.prepare("SELECT id, text FROM notes;") catch err { return 5; };
while q.step() ?? false {
    io::print_i64(q.int(0));
    io::println(q.text(1));
}
q.finish();
db.close();
```

## Use it

```
fels add sqlite github.com/fels-lang/sqlite
```

The package's `build.fels` gives your programs the library folder and copies `sqlite3.dll`
next to them, for `fels build` and for the test programs of `fels test`.

## The API

| | |
| --- | --- |
| `sqlite::open(path)` | open or create a database file; `":memory:"` for one in memory |
| `sqlite::open_read_only(path)` | open an existing file only to read it |
| `db.exec(sql)` | run one or more statements that return no rows |
| `db.prepare(sql)` | a `Stmt` for one statement |
| `db.message()` | what went wrong last, in SQLite's words |
| `db.changes()`, `db.last_rowid()` | rows the last change touched, the rowid of the last INSERT |
| `db.wait_for_locks(ms)` | wait for a lock before failing with `busy` |
| `db.close()` | close; finish the statements first |
| `st.bind_int`, `bind_float`, `bind_text`, `bind_null` | a value for parameter `?N`, counted from 1 |
| `st.step()` | true when a row is there, false when the statement is done |
| `st.int(col)`, `float(col)`, `text(col)`, `is_null(col)`, `columns()` | the row, columns counted from 0 |
| `st.reset()`, `st.finish()` | run again with the same values; free the statement |
| `sqlite::version()` | "3.54.0" |

Errors are `SqlError`: `cannot_open`, `busy`, `constraint`, `misuse` and `failed`. Text from
`st.text` is valid until the statement's next `step`, `reset` or `finish`; copy it to keep it.

## Work on it

```
fels test        run the tests (src/sqlite_tests.fels; they call SQLite)
fels build run   build and run examples/demo.fels
```

`lib/README.md` says how the DLL was built.
