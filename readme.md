# pg-dump-restore-node-wrapper

This very small utility that includes pg_dump and pg_restore binaries for Windows and macOS, taken from Postgres 13.4. DLLs and dylibs are also included, making the binaries standalone.

A thin CLI wrapper is also provided.

## Usage

```js
import pgDumpRestore from "pg-dump-restore-node-wrapper";

async function main() {

  const { stdout, stderr } = await pgDumpRestore.dump({
    port, // defaults to 5432
    host,
    dbname,
    username,
    password,
    version, // defaults to '17rc1'
    file: "./test.pgdump",
    format, // defaults to 'c'
  }); // outputs an execa object

  const { stdout, stderr } = await pgDumpRestore.restore({
    port, // defaults to 5432
    host,
    dbname,
    username,
    password,
    version, // defaults to '17rc1'
    filename: "./test.pgdump", // note the filename instead of file, following the pg_restore naming.
    disableTriggers, // defaults to false
    clean, // defaults to false
    create, // defaults to false  
    createWith: `TEMPLATE=template0 ENCODING='UTF8' LC_COLLATE='en-US' LC_CTYPE='en-US';` // optional (only if create is true)
  }); // outputs an execa object
 
}
```

Please see the [pg_dump](https://www.postgresql.org/docs/current/app-pgdump.html) and [pg_restore](https://www.postgresql.org/docs/current/app-pgrestore.html) documentation for details on the arguments and [execa](https://github.com/sindresorhus/execa) for details on the output streams.
 
### macOS

I used macdylibbundler from https://github.com/SCG82/macdylibbundler to list all dll files and I then copied and renamed them.

I did not use the tool to bundle the dependencies as it would corrupt the executable.

### Windows

I used https://github.com/lucasg/Dependencies to determine the required DLL, filtering only the local dependencies.
 
## To publish to GHCR

Add this to `package.json`
```json
  "publishConfig": {
    "registry":"https://npm.pkg.github.com"
  },
```

and add a `.npmrc` to the root of the repo with 
```.npmrc
//npm.pkg.github.com/:_authToken=AUTH_TOKEN
```
Where `AUTH_TOKEN` is replaced with a GitHub token.