# RocketMQ-Client-CPP #

RocketMQ-Client-CPP is the C/C++ client of Apache RocketMQ which is a distributed messaging and streaming platform with low latency, high performance and reliability, trillion-level capacity and flexible scalability.

## Features ##

- produce message, include sync and asynce produce message, timed and delay message. 

- consume message, include concurrency and orderly consume message, broadcast and cluster consume model.

- rebalance, include both produce message and consume message with rebalance.

- C and C++ API, include both C style and C++ style apis.

- across platform, all features are supported on both windows and linux system.

- low latency, publish latency < 2ms, subscribe latency < 10ms

- high tps, rocketmq topic with 16 message queues, stand-alone cpp client can reach publish TPS > 3w and subsricbe TPS > 15w.

- reliability, based on nameServer snapshot and network disaster recovery strategy, no real-time impact on publish and subscribe when anyone of broker or nameSrv was broken.

## Dependency ##
- libevent 2.0.22

- jsoncpp 0.10.6

- boost 1.56.0

## Documentation ##
doc/rocketmq-cpp_manaual_zh.docx

## Build and Install ##

### Linux platform ###

**note**: *make sure the following compile tools or libraries has been installed before install dependency libraries*

- compile tools:  **gcc-c++**、**cmake**、**automake**、**libtool**

- libraries:   **openssl-devel**、**bzip2-devel**

#### Dependency Installation ####

##### 1. install [libevent 2.0.22](https://github.com/libevent/libevent/archive/release-2.0.22-stable.zip "libevent 2.0.22")
```shell
./autogen.sh
./configure
make
make install
```

##### 2. install [JsonCPP 0.10.6](https://github.com/open-source-parsers/jsoncpp/archive/0.10.6.zip  "jsoncpp 0.10.6")
```shell
cmake .
make
make install
```

##### 3. install [boost 1.56.0](http://sourceforge.net/projects/boost/files/boost/1.56.0/boost_1_56_0.tar.gz "boost 1.56.0")
```shell
./bootstrap.sh
./b2 link=shared runtime-link=shared cxxflags=" -fPIC"
./b2 install
```
#### 2. Make and Install ####
```shell
cmake .
make
make install
```

### Windows platform ###
**note**: *make sure the following compile tools or libraries has been installed before install dependency libraries*

- compile tools:  **CMake**、**Visusal Studio 2013**

- libraries:   **zlib**
#### Dependency Installation
##### 1. install [libevent 2.0.22](https://github.com/libevent/libevent/archive/release-2.0.22-stable.zip "libevent 2.0.22")
Extract libevent to LocalPath
copy files in [libevent-2.0.22-stable\WIN32-Code\] to [libevent-2.0.22-stable\include\]
Open Visual Studio command line tools(x64)
```shell
cd libevent-2.0.22-stable
nmake /f Makefile.nmake
dir *.lib
mkdir lib
copy libevent.lib lib\
copy libevent_core.lib lib\
copy libevent_extras.lib lib\
```
##### 2. install [jsoncpp 0.10.6](https://github.com/open-source-parsers/jsoncpp/archive/0.10.6.zip "jsoncpp 0.10.6")
Extract jsoncpp to LocalPath
```shell
1.Open Visual Studio with [jsoncpp-0.10.6\makefiles\msvc2010\jsoncpp.sln]
  Then perheps there is a message box popup, click OK.
2.Set [Configuration]:Release at Toolbar
  Set [Platform]:x64 at Toolbar
3.Right click project [lib_json], and click [build], 
  and then Visual Studio will compile the lib file at [x64/Release]
4.copy [jsoncpp-0.10.6\makefiles\msvc2010\x64\Release\lib_json.lib]
  to [jsoncpp-0.10.6\lib\lib_json.lib]
```
##### 3. install [boost 1.56.0](http://sourceforge.net/projects/boost/files/boost/1.56.0/boost_1_56_0.zip "boost 1.56.0") and [zlib 1.2.3](http://gnuwin32.sourceforge.net/downlinks/zlib-src-zip.php "zlib 1.2.3")
Extract [boost 1.56.0] and [zlib] to LocalPath
modify "#if 1" to "#if HAVE_UNISTD_H" at [zlib-1.2.3-src\src\zlib\1.2.3\zlib-1.2.3\zconf.h]
Open Visual Studio command line tools(x64)
```shell
cd boost_1_56_0
booststrap.bat
bjam.exe --toolset=msvc-12.0 architecture=x86 address-model=64 link=static runtime-link=static stage --stagedir="[Your Loacl Path]\boost_1_56_0\lib" threading=multi variant=release --build-type=complete --with-iostreams -s ZLIB_SOURCE=[Your Loacl Path]\zlib-1.2.3-src\src\zlib\1.2.3\zlib-1.2.3
```
[Your Loacl Path] is the directory that you extract the zip file. 

#### Make and Install
##### 1.get source
```shell
git clone https://github.com/apache/rocketmq-client-cpp.git
```
##### 2. configure path
open the CMakeLists.txt at [rocketmq-client-cpp] and modify the config.
```shell
set(BOOST_INCLUDEDIR           [Your Local Path]/boost_1_56_0/)

set(LIBEVENT_INCLUDE_DIR       [Your Local Path]/libevent-2.0.22-stable/include/)
set(LIBEVENT_CORE_LIBRARY      [Your Local Path]/libevent-2.0.22-stable/lib/libevent_core.lib)
set(LIBEVENT_EXTRAS_LIBRARY    [Your Local Path]/libevent-2.0.22-stable/lib/libevent_extras.lib)
set(LIBEVENT_LIBEVENT_LIBRARY  [Your Local Path]/libevent-2.0.22-stable/lib/libevent.lib)

set(JSONCPP_INCLUDE_DIRS       [Your Local Path]/jsoncpp-0.10.6/include/)
set(JSONCPP_LIBRARIES          [Your Local Path]/jsoncpp-0.10.6/lib/lib_json.lib)
```
##### 3. download [CMake Tool](https://cmake.org/files/v3.13/cmake-3.13.0-rc3-win64-x64.zip "cmake-3.13.0-rc3")
Extract cmake to LocalPath.
Run cmake-gui.exe, choose your source code dir and build dir, then click generate which will let you choose [Virtual Studio 12 2013 Win64] version.

##### 4. build 
Use Visual Studio open the Project at [rocketmq-client-cpp] and then build the project.


## Quick Start ##
### tools and commands ###

- sync produce message
```shell
./SyncProducer -g group1 -t topic1 -c test message -n 172.168.1.1:9876
```
- async produce message
```shell
./AsyncProducer -g group1 -t topic -c test message -n 172.168.1.1:9876
```
- send delay message
```shell
./SendDelayMsg -g group1 -t topic -c test message -n 172.168.1.1:9876
```
- sync push consume message
```shell
./PushConsumer -g group1 -t topic -c test message -s sync -n 172.168.1.1:9876 
```
- async push comsume message
```shell
./AsyncPushConsumer -g group1 -t topic -c test message -n 172.168.1.1:9876
```
- orderly sync push consume message
```shell
./OrderlyPushConsumer -g group1 -t topic -c test message -s sync -n 172.168.1.1:9876
```
- orderly async push consume message
```shell
./OrderlyPushConsumer -g group1 -t topic -c test message -n 172.168.1.1:9876
```
- sync pull consume message
```shell
./PullConsumer -g group1 -t topic -c test message -n 172.168.1.1:9876
```
### Parameters for tools ###
```bash
-n	: nameserver addr, format is ip1:port;ip2:port
-i	: nameserver domain name, parameter -n and -i must have one.
-g	: groupName, contains producer groupName and consumer groupName
-t	: message topic
-m	: message count(default value:1)
-c	: message content(default value: only test)
-b	: consume model(default value: CLUSTER)
-a	: set sync push(default value: async)
-r	: setup retry times(default value:5 times)
-u	: select active broker to send message(default value: false)
-d	: use AutoDeleteSendcallback by cpp client(defalut value: false)
-T	: thread count of send msg or consume message(defalut value: system cpu core number)
-v	: print more details information
```
