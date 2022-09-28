---
layout: post
title:  "Maven Cheat Sheet"
date:   2021-02-01 00:00:00 +0800
categories: java maven
---

## Phases
Reference to [maven lifecycle](https://maven.apache.org/guides/introduction/introduction-to-the-lifecycle.html)
Pay interest to:
- default lifecycle: all phases availaable
- built-in lifecycle bindings: preset phases sequence depending the packaging
- **Goal** and **Phase** are different concepts, goals are attached to phase or ca be called directly
  
## Useful Commands
Know plugin available commands
```sh
mvn help:describe -Dplugin=<groupId>:<artifactId> -Ddetail
```

Download sources and javadocs
```sh
mvn eclipse:eclipse -DdownloadSources -DdownloadJavadocs
```

Install locally a dependency
```sh
mvn install:install-file  -Dfile=ojdbc6.jar  -DgroupId=com.oracle -DartifactId=ojdbc6 -Dversion=11.2.0.1.0 -Dpackaging=jar
```
