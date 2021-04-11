---
layout: post
title: How to set JAVA_HOME in Mac permanently?
image: https://i.stack.imgur.com/EIi48.jpg
description: >
  I am trying to set `JAVA_HOME` by entering export `JAVA_HOME=/Library/Java/Home` at terminal. 
hide_description: true
hide_image: true
comments: true
more_posts: posts.md
featured: true
last_modified_at: 2021-04-11
categories: [stackoverflow, mac]
tags: [java, macos, java-home]
---

How to set JAVA_HOME in Mac permanently?

{:.no_toc}
1. this unordered seed list will be replaced by toc as unordered list
{:toc}

## [Question](https://stackoverflow.com/questions/14702702/how-to-set-java-home-in-mac-permanently) by [Vishal Tavande](https://stackoverflow.com/users/2009908/vishal-tavande)

I am trying to set `JAVA_HOME` by entering export `JAVA_HOME=/Library/Java/Home` at terminal. It sets the `JAVA_HOME` for current session.

How can I set it permanently?


## [Answer](https://stackoverflow.com/a/65162351/13155046) by [Milovan Tomašević](https://stackoverflow.com/users/13155046/milovan-tomašević)

Installing Java on `macOS 11 Big Sur`:
- the easiest way is to select OpenJDK 11 (LTS), the HotSpot JVM, and macOS x64 is to get the latest release here: [adoptopenjdk.net][1]
- Select macOS and x64 and download the `JDK` (about 190 MB), which will put the `OpenJDK11U-jdk_x64_mac_hotspot_11.0.9_11.pkg` file into your `~/Downloads folder`
- Clicking on pkg file, will install into this location: `/Library/Java/JavaVirtualMachines/adoptopenjdk-11.jdk`

[![enter image description here][2]][2]

- Almost done. After opening a terminal, the successful installation of the JDK can be confirmed like so: `java --version`
   - output:

```sh
openjdk 11.0.9.1 2020-11-04
OpenJDK Runtime Environment AdoptOpenJDK (build 11.0.9.1+1)
OpenJDK 64-Bit Server VM AdoptOpenJDK (build 11.0.9.1+1, mixed mode)
```

- `JAVA_HOME` is an important environment variable and it’s important to get it right. Here is a trick that allows me to keep the environment variable current, even after a Java Update was installed. In `~/.zshrc`, I set the variable like so: `export JAVA_HOME=$(/usr/libexec/java_home)`
- In previous macOS versions, this was done in `~/.bash_profile`. Anyway, open a new terminal and verify: `echo $JAVA_HOME`
   - output: `/Library/Java/JavaVirtualMachines/adoptopenjdk-11.jdk/Contents/Home`

***
TEST: Compile and Run your Java Program

- Open a text editor, copy the code from below and save the file as `HelloStackoverflow.java`. 
```java
// file: 'test.java'
public class HelloStackoverflow {
  public static void main(String[] args){
    System.out.println("Hello Stackoverflow !");
  }//End of main
}//End of HelloStackoverflow Class
```

- From a terminal set the working directory to the directory containing `HelloStackoverflow.java`, then type the command:

```sh
# file: 'terminal'
javac HelloStackoverflow.java
```

- If you're lucky, nothing will happen

- Actually, a lot happened. `javac` is the name of the Java compiler. It translates Java into `Java Bytecode`, an assembly language for the Java Virtual Machine (JVM). The Java Bytecode is stored in a file called `HelloStackoverflow.class`.

- Running: type the command:

~~~sh
# file: 'terminal'
java HelloStackoverflow

# output:
# Hello Stackoverflow !
~~~


[![enter image description here][3]][3]


  [1]: https://adoptopenjdk.net/archive.html?variant=openjdk11&jvmVariant=hotspot
  [2]: https://i.stack.imgur.com/EIi48.jpg
  [3]: https://i.stack.imgur.com/dp640.png