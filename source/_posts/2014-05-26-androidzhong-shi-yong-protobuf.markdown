---
layout: post
title: "Android中使用protobuf"
date: 2014-05-26 23:07:52 +0800
comments: true
categories: Android
---
Android中使用protobuf
=========

Protobuf是Google开发的一种数据描述语言，大家可以参照json和xml来进行理解。但是相对于json和xml，Protobu的传输内容和解析熟读却有着极大的优势。

Protobuf分为complier（用于生成语言相关文件）和runtimelib（嵌入到程序中解析收到的数据）两个库，我们需要使用complier编译生成所需要的语言的文件，比如Android中我们就需要生成一个.java文件。然而标准protobuf的runtimelib大到700kb左右，所以Android开发团队提供了一套mircro的compiler和runtimelib。

###本文环境

Ubuntu12.04

###编译生成标准protobuf
```
$ git clone https://android.googlesource.com/platform/external/protobuf
$ cd protobuf/
$ ./configure
$ make
```
之后在protobuf/src目录下会有一个protoc文件生成，最好生成软链
```
$ cd /usr/local/bin
$ sudo ln -s path/to/protobuf/src/protoc/ protoc
```
可以使用
```
$ protoc -h
```
进行检测


> ……

> --cpp_out=OUT_DIR           Generate C++ header and source.

> --java_out=OUT_DIR          Generate Java source file.

> --javamicro_out=OUT_DIR     Generate Java source file micro runtime.

> --javanano_out=OUT_DIR      Generate Java source file nano runtime.

> --python_out=OUT_DIR        Generate Python source file.


如上可以看到

--javamicro_out=OUT_DIR Generate Java source file micro runtime.

--javanano_out=OUT_DIR Generate Java source file nano runtime.

就说明编译成功。

接下来将protobuf/java/src/main/java/com/google/protobuf/micro的代码拷贝到开发环境中，这个就是protbuf的rubtimelib，也可以下载protobuf-micro.jar。

编写一个名为entity.proto的文件，假设内容为

```
message Person {
    required string name=1;
    required int32 id=2;
    optional string email=3;

    enum PhoneType {
        MOBILE=0;
        HOME=1;
        WORK=2;
    }

    message PhoneNumber {
        required string number=1;
        optional PhoneType type=2 [default=HOME];
    }

    repeated PhoneNumber phone=4;
}
```
然后使用
```
$ protoc --javamicro_out=. entity.proto
```
生成文件，把生成的Entity.java拷贝到工程目录下。
```
// 生成要byte数据
Entity.Person person = new Entity.Person();
byte[] personBytes = person.toByteString();

// 解析byte数据
Entity.Person parsedPerson = Entity.Person.parseFrom(psersonBytes);

```

PS
----

第一次使用protobuf的时候选择了[wire](https://github.com/square/wire)，
这是一个由square公司开发的应用于Android的protobuf的库。在使用过程中发现wire无法混淆，这在一些需要对自己的通讯协议进行加密的情况下不是很适用。

    
    
