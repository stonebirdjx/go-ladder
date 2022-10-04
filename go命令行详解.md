<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [查看帮助](#%E6%9F%A5%E7%9C%8B%E5%B8%AE%E5%8A%A9)
- [go bug - 上报错误](#go-bug---%E4%B8%8A%E6%8A%A5%E9%94%99%E8%AF%AF)
- [:point_right:go build - 编译包和依赖](#point_rightgo-build---%E7%BC%96%E8%AF%91%E5%8C%85%E5%92%8C%E4%BE%9D%E8%B5%96)
- [go vet - 静态检查](#go-vet---%E9%9D%99%E6%80%81%E6%A3%80%E6%9F%A5)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# 查看帮助

go --help

go help <command>获取有关命令的更多信息。 

# go bug - 上报错误

```bash
go bug
```

# :point_right:go build - 编译包和依赖

```go
```



# go vet - 静态检查

```go
go vet [-n] [-x] [-vettool prog] [build flags] [vet flags] [packages]

go vet vet.go
```



​        build       compile packages and dependencies
​        clean       remove object files and cached files
​        doc         show documentation for package or symbol
​        env         print Go environment information
​        fix         update packages to use new APIs
​        fmt         gofmt (reformat) package sources
​        generate    generate Go files by processing source
​        get         add dependencies to current module and install them
​        install     compile and install packages and dependencies
​        list        list packages or modules
​        mod         module maintenance
​        work        workspace maintenance
​        run         compile and run Go program
​        test        test packages
​        tool        run specified go tool
​        version     print Go version
​        vet         report likely mistakes in packages