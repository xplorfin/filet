[![Coverage Status](https://coveralls.io/repos/github/xplorfin/filet/badge.svg?branch=master)](https://coveralls.io/github/xplorfin/filet?branch=master)
[![Renovate enabled](https://img.shields.io/badge/renovate-enabled-brightgreen.svg)](https://app.renovatebot.com/dashboard#github/xplorfin/filet)
[![Build status](https://github.com/xplorfin/filet/workflows/test/badge.svg)](https://github.com/xplorfin/filet/actions?query=workflow%3Atest)
[![Build status](https://github.com/xplorfin/filet/workflows/goreleaser/badge.svg)](https://github.com/xplorfin/filet/actions?query=workflow%3Agoreleaser)
[![GoDoc](https://godoc.org/github.com/xplorfin/filet?status.svg)](https://godoc.org/github.com/xplorfin/filet)
[![Go Report Card](https://goreportcard.com/badge/github.com/xplorfin/filet)](https://goreportcard.com/report/github.com/xplorfin/filet)


# Filet 🍖
A small temporary file utility for Go testing. Built on [Afero](https://github.com/spf13/afero) and heavily inspired by the way Afero tests itself.

This is a fork of [this library](https://github.com/Flaque/filet) maintained by [entropy](https://entropy.rocks)

Install via:
`$ go get github.com/xplorfin/filet`

Then import with:
```
import (
  filet "github.com/xplorfin/filet"
)
```

Quick overview at [GoDocs](https://godoc.org/github.com/xplorfin/filet).

## Creating temporaries

### Creating temporary directories:
```
func TestFoo(t *testing.T) {
  filet.TmpDir(t, "") // Creates a temporary dir with no parent directory
  filet.TmpDir(t, "myPath") // Creates a temporary dir at `myPath`
}
```

### Creating temporary files:
```
func TestFoo(t *testing.T) {
  filet.TmpFile(t, "", "") // Creates a temporary file with no parent dir

  // Creates a temporary file with string "some content"
  filet.TmpFile(t, "", "some content")

  // Creates a temporary file with string "some content"
  filet.TmpFile(t, "myDir", "some content")
}
```

### Creating specified files:
```
func TestFoo(t *testing.T) {
  filet.File(t, "conf.yaml", "") // Creates a specified file

  // Creates a specified file with string "some content"
  filet.File(t, "/tmp/conf.yaml", "some content")
}
```

### Cleaning up after yourself:
Filet lets you clean up after your files with `CleanUp`, which is
most cleanly used at the beginning of a function with `defer`. For example:

```
func TestFoo(t *testing.T) {
  defer filet.CleanUp(t)

  // Create a bunch of temporary stuff here
}
```

`CleanUp` will call `t.Error` if something goes wrong when removing the file.

You can also access the `Files` itself if you want to add a specificly
named file to the cleanup list.

```
filet.Files = append(filet.Files, "path/to/my/named/file")
```

## Helpers

Filet comes with a few helper functions that are useful for working with your
temporary files.

### Checking Existence
You can test if a file exists if you want.
```
func TestFoo(t *testing.T) {
  myBool := filet.Exists(t, "path/to/my/file")
}
```

### Checking DirContains
You can test if a folder contains a file or another directory.
```
func TestFoo(t *testing.T) {
  myBool := filet.DirContains(t, "path/to/myFolder", "myFile")
}
```

### Checking if a FileSays what you want
You can check if a file's contents says what you want it to with `FileSays`.

```
func TestFoo(t *testing.T) {
  myBool := filet.FileSays(t, "path/to/my/dir", []byte("my content here"))
}
```
