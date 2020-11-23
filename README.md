# numfile

Automatically assign an increasing sequence number to file names.

## Installation

```shell
$ python -m pip install numfile
```

## Use Cases

Write data to numbered snapshot files in a given directory:

```shell
$ python -q
>>> from numfile import open_next
>>> for version in ("first", "second", "third"):
...     with open_next("/tmp/snapshot.txt") as file:
...         print(f"The {version} version.", file=file)
...

$ ls -d /tmp/* | grep snapshot
/tmp/snapshot-1.txt
/tmp/snapshot-2.txt
/tmp/snapshot-3.txt
```

Append messages to the latest log file:

```shell
$ ls -tr | grep app-error
app-error.log
app-error-2.log
app-error-3.log

$ python -q
>>> from numfile import open_latest
>>> with open_latest("app-error.log") as file: 
...     print("Oops... Something went wrong!", file=file)
...     print(file.name)
...
app-error-3.log

$ tail -n1 app-error-3.log
Oops... Something went wrong!
```

Consolidate similar files in chronological order:

```shell
$ python -q
>>> from numfile import open_all
>>> for file in open_all("/tmp/snapshot.txt"): 
...     print(file.name, file.read(), end="")
...
/tmp/snapshot-1.txt The first version.
/tmp/snapshot-2.txt The second version.
/tmp/snapshot-3.txt The third version.
```

## Development

```shell
$ git clone git@github.com:bzaczynski/numfile.git
$ cd numfile
$ pyenv local 3.9.0
$ python -m venv .venv --prompt=numfile
$ source .venv/bin/activate
$ pip install -U pip flit
$ flit install --deps=all --symlink
$ python -m pytest
```

## Releasing

Bump the version:

```shell
$ bump2version minor
$ git status
```
