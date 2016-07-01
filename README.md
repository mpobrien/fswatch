# fswatch
[![Build Status](https://travis-ci.org/codeskyblue/fswatch.svg?branch=master)](https://travis-ci.org/codeskyblue/fswatch)
[![Build status](https://ci.appveyor.com/api/projects/status/hble6an55u4a04e5/branch/master?svg=true)](https://ci.appveyor.com/project/codeskyblue/fswatch/branch/master)

**fswatch** is a developer tool that triggers commands in response to filesystem changes.
Works well on Mac, Linux, and should also works on Windows(not well tested).

## Install
```
go get -u -v github.com/codeskyblue/fswatch
```
OR
```
curl -s https://raw.githubusercontent.com/franciscocpg/fswatch/master/install.sh | sudo sh
```

## Quick start
### Step 1
Create a config file `.fsw.yml`, quickly generated by the following command.

	fswatch init

config file example

```
desc: Auto generated by fswatch [fswatch]
triggers:
- pattens:
  - '**/*.go'
  - '**/*.c'
  # also support '!**/test_*.go'
  env:
    DEBUG: "1"
  # if shell is true, $cmd will be wrapped with `bash -c`
  shell: true
  cmd: go test -v
  delay: 100ms
  signal: "KILL"
watch_paths:
- .
watch_depth: 5
```

### Step 2
Run `fswatch` directly.
Every time you edit a file. Command `go test -v` will be called.

```
$ fswatch
fswatch >>> exec start: go test -v
# github.com/codeskyblue/fswatch
./fswatch.go:281: main redeclared in this block
	previous declaration at ./config.go:354
fswatch >>> program exited: exit status 2
fswatch >>> finish in 145.499911ms
```



## You should know
### How fswatch kill process
fswatch send signal to all process when restart. (mac and linux killed by pgid, windows by taskkill)
`Ctrl+C` will trigger fswatch quit and kill all process it started.

### Pattens
More about the pattens. The patten has the same rule like `.gitignore`.
So you can write like this.

```
- pattens:
  - '*.go'
  - '!*_test.go'
  - '!**/bindata.go'
```

`main.go` changed will trigger command, but `a_test.go` and `libs/bindata.go` will be ignored.

## FAQs
`too many open files`

For mac, run the following command

    sysctl -w kern.maxfiles=20480
    sysctl -w kern.maxfilesperproc=18000
    ulimit -S -n 2048

[reference](http://superuser.com/questions/433746/is-there-a-fix-for-the-too-many-open-files-in-system-error-on-os-x-10-7-1)

## Other


Chinese Blog: <http://my.oschina.net/goskyblue/blog/194240>

## Friendly link: 
* [bee](https://github.com/astaxie/bee)
* [fsnotify](github.com/go-fsnotify/fsnotify)
* <https://github.com/codeskyblue/kexec>
* <https://github.com/codeskyblue/dockerignore>

## Code History
I reviewed the first version of fswatch(which was taged 0.1). The code I look now is shit. So I deleted almost 80% code, And left some very useful functions.

## Alternative
* <https://github.com/cortesi/modd>
* <https://github.com/emcrisostomo/fswatch> write with cpp
* <https://github.com/Unknwon/bra> Funny name.

## LICENSE
Under [MIT](LICENSE)
