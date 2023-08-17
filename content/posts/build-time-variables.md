---
title: "Build Time Variables"
date: 2023-08-16T20:44:54-04:00
draft: true
categories: ["Tiny Learnings", "Development", "Go"]
---

While not entirely new learning, I spent time today working on variable injection using ldflags with `go build` - the Gopher Guide folks have an article on Digital Ocean to learn about using [ldflags for setting version information](https://www.digitalocean.com/community/tutorials/using-ldflags-to-set-version-information-for-go-applications)

Until today I always thought variable injection worked for variables defined in a Go program's main package. In needing to inject a version string held by an exported `Version` variable in a `cmd` package, I learned the way to reference the variable within the ldflags is to use the full import path of the cmd package.  

```

go build -- ldflags '-X github.com/example/cmd.Version=X.Y.Z'
`````

For context, the ldflags mentioned in this post are part of a goreleaser build pipeline for setting the binary version to the value of a git tag. 

