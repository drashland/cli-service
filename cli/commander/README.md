# Commander

A service to help build command-line interfaces (CLIs).

## Table of Contents

* [Quickstart](#quickstart)

## Quickstart

1. Create your `app.ts` file.

```
import {
  Commander,
  Subcommand,
} from "https://raw.githubusercontent.com/drashland/services/v0.2.0/cli/mod.ts";

const decoder = new TextDecoder();

class Read extends Subcommand {
  public signature = "read [file]";
  public description = "Read a file.";

  public async handle(): Promise<void> {
    const file = this.getArgument("[file]");
    const contents = Deno.readFileSync(file);
    console.log(decoder.decode(contents));
  }
}

class Write extends Subcommand {
  public signature = "write [file] [contents]";
  public description = "Write contents to a file.";

  public async handle(): Promise<void> {
    const file = this.getArgument("[file]");
    const contents = this.getArgument("[contents]");
    try {
      Deno.writeFileSync(file, contents);
      console.log("Successfully wrote file.");
    } catch (error) {
      console.log(error);
    }
  }
}

const service = new Commander({
  command: "fm",
  name: "File Manager",
  description: "A file manager.",
  version: "v1.0.0",
  subcommands: [
    Read,
    Write,
  ],
});

service.run();
```

2. Install your `app.ts` file as a binary under the name `fm`.

```
$ deno install --allow-read --allow-write --name fm app.ts
```

3. Run your app.

```
$ fm

File Manager - A file manager.

USAGE

    fm [option | [[subcommand] [args] [deno flags] [options]]

OPTIONS

    -h, --help    Show this menu.
    -v, --version Show this CLI's version.

SUBCOMMANDS

    read
        Read a file.
    write
        Write contents to a file.
```
