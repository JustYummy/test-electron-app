# test-electron-app

### Use `electron`@11.2.0, and `@journeyapps/sqlcipher`5.0.0

In the `background.ts`, I pasted the testing code and run

```typescript
// background.ts
function testSqlite() {
  const sqlite3 = sqliteBase.verbose();
  const db = new sqlite3.Database('test.db');

  db.serialize(() => {
    // This is the default, but it is good to specify explicitly:
    db.run('PRAGMA cipher_compatibility = 4');

    // To open a database created with SQLCipher 3.x, use this:
    // db.run("PRAGMA cipher_compatibility = 3");

    db.run("PRAGMA key = 'mysecret'");
    db.run('CREATE TABLE lorem (info TEXT)');

    const stmt = db.prepare('INSERT INTO lorem VALUES (?)');
    for (let i = 0; i < 10; i += 1) {
      stmt.run(`Ipsum ${i}`);
    }
    stmt.finalize();

    db.each('SELECT rowid AS id, info FROM lorem', (err: Error, row: any) => {
      console.log(`${row.id}: ${row.info}`);
    });
  });

  db.close();
}

testSqlite();
```

on MacOS,  It runs ok, but on the Windows, It crash. 

The Node version on Windows is `v14.15.4`

## Project setup
```
yarn install
```

### Run on Electron
```
yarn electron:serve
```


