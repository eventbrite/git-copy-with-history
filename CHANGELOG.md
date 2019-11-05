# Changelog

## 1.1.4

- Switch back to `git-am` since `git-apply` does not commit. Leaving the loop in place

## 1.1.3

- Moved away from `git-am` to `git-apply` and run each patch individually
  (This caused issues when copying a directory with a large
  commit history)

## 1.1.2

- Update to use system temp dir instead of custom in current working directory

## 1.1.1

- [72e3280](https://github.com/kwelch-eb/git-copy-with-history/commit/72e3280fd0ddab0a4f02e4fb343137c45c439a04)Fix bug in get absolutely path

## [1.1.0](https://github.com/kwelch-eb/git-copy-with-history/commit/8eb46b0076f51815ae22483fc945ba9736f4baeb)

- [1a2677d](https://github.com/kwelch-eb/git-copy-with-history/commit/1a2677d5a5c6d0abb6a32661b0c7963c30a86a9a)Updated to work for copying root repository into a folder of a another repository

## [1.0.0](https://github.com/kwelch-eb/git-copy-with-history/commit/96733fe922abeb7be6f2b820def67bf5d41b09e7)

Initial Release
