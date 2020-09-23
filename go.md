| 变量   | 说明                                                         |
| ------ | ------------------------------------------------------------ |
| GOROOT | go安装路径                                                   |
| GOPATH | 开发工作区,用于存放源代码,可执行文件等,GOPATH可以设置多个目录,GOPATH一般分为三个目录,bin为可执行文件,src为源代码目录,pkg为库静态文件, GOROOT和GOPATH路径不可相同 |
| GOBIN  | 编译后可执行文件目录,使用go install命令会将编译打包好的文件放进GOBIN目录,通常GOBIN目录为 GOPATH/bin |





```bash
go env # 打印所有go环境变量
```



```bash
go env GOROOT # 打印go某个环境变量
```



```bash
go env GOOS GOARCH # 获取go内置变量的值
```



| $GOOS | $GOARCH |
| ----- | ------- |
| android   | arm      |
| darwin    | 386      |
| darwin    | amd64    |
| darwin    | arm      |
| darwin    | arm64    |
| dragonfly | amd64    |
| freebsd   | 386      |
| freebsd   | amd64    |
| freebsd   | arm      |
| linux     | 386      |
| linux     | amd64    |
| linux     | arm      |
| linux     | arm64    |
| linux     | ppc64    |
| linux     | ppc64le  |
| linux     | mips     |
| linux     | mipsle   |
| linux     | mips64   |
| linux     | mips64le |
| linux     | s390x    |
| netbsd    | 386      |
| netbsd    | amd64    |
| netbsd    | arm      |
| openbsd   | 386      |
| openbsd   | amd64    |
| openbsd   | arm      |
| plan9     | 386      |
| plan9     | amd64    |
| solaris   | amd64    |
| windows   | 386      |
| windows   | amd64    |