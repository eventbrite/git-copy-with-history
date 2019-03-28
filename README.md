# git-clone-with-history

A git plugin that will copy a directory to a new location with each files commit history.

## Installation

```
git clone https://github.com/kwelch-eb/git-copy-with-history.git
make install
```

This will install the plugin into `usr/local/bin` which is `git` will pick up any plugins in path

## Usage

`git copy-with-history <source_path> <destination_path>`

The paths can be absolute or relative.

## Further Reading

- [How it works](./how-it-works.md) - Goes into the process of finding this solution