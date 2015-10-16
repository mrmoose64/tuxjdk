# Introduction #
[enable-64bit-client.diff](https://code.google.com/p/tuxjdk/source/browse/quilt-patches/enable-64bit-client.diff)

Patch removes conditional statements in makefiles to allow Client VM to be built for x86\_64 architecture.

# Details #

### Client vs Server ###
There are 2 main types of VM: client and server. As name suggests, client is made for user-oriented desktop apps, and server is made for long-running servers. They have different memory management strategies and performance strategies: client VM tried to keep small memory footprint and give best possible performance at once, while server consumes all available memory and uses profiling hotspots to optimize performance over time, sacrificing initial VM performance.

### 64 bit issue ###
For some reason, Sun, and now Oracle, does not want to support Client VM for i586. So on x64 builds of JRE and JDK only Server VM is available. According to OpenJDK developer mailing lists, there is no technical limitation and Client VM should work just fine for x64. But there is a number of conditional statements in makefiles to check architecture and disable build of Client VM for x64.

### Patch internals ###
The only thing changed by the patch - remove limitation in makefiles. No source code changes are performed whatsoever.

# Benchmarks to consider #
All of the following tests are done on Oracle JDK 1.8.0 for i586.

### Cold start time, NetBeans 7.4 ###
| Client | ~11s |
|:-------|:-----|
| Server | ~23s |

### Cold start memory footprint, NetBeans 7.4 ###
| Client | ![https://googledrive.com/host/0B68yuEpDuq6wMTBpazBSTnZKWnM/clientvm/nb-7.4-cold-client.png](https://googledrive.com/host/0B68yuEpDuq6wMTBpazBSTnZKWnM/clientvm/nb-7.4-cold-client.png) |
|:-------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  Server | ![https://googledrive.com/host/0B68yuEpDuq6wMTBpazBSTnZKWnM/clientvm/nb-7.4-cold-server.png](https://googledrive.com/host/0B68yuEpDuq6wMTBpazBSTnZKWnM/clientvm/nb-7.4-cold-server.png) |

### Cold start memory footprint, Eclipse 3.6.2 ###
| Client | ![https://googledrive.com/host/0B68yuEpDuq6wMTBpazBSTnZKWnM/clientvm/eclipse-client.png](https://googledrive.com/host/0B68yuEpDuq6wMTBpazBSTnZKWnM/clientvm/eclipse-client.png) |
|:-------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Server | ![https://googledrive.com/host/0B68yuEpDuq6wMTBpazBSTnZKWnM/clientvm/eclipse-server.png](https://googledrive.com/host/0B68yuEpDuq6wMTBpazBSTnZKWnM/clientvm/eclipse-server.png) |

### Huge project memory footprint, OpenJDK in NetBeans 8.0 ###
| Client | ![https://googledrive.com/host/0B68yuEpDuq6wMTBpazBSTnZKWnM/clientvm/nb-process-memory-client.png](https://googledrive.com/host/0B68yuEpDuq6wMTBpazBSTnZKWnM/clientvm/nb-process-memory-client.png) |
|:-------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Server | ![https://googledrive.com/host/0B68yuEpDuq6wMTBpazBSTnZKWnM/clientvm/nb-process-memory-server.png](https://googledrive.com/host/0B68yuEpDuq6wMTBpazBSTnZKWnM/clientvm/nb-process-memory-server.png) |