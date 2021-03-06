---
title: "Enter the JVM: Part 1"
date: 2018-04-30T00:35:05+05:30
draft: false
tags: ["programming", "jvm", "java", "clojure"]
---

After [deciding to learn about the JVM](/blog/visit-the-old-to-understand-the-new/), I started reading the [JVM specification](https://docs.oracle.com/javase/specs/jvms/se10/html/index.html). I felt the need for trying out few things on my machine to get some practical idea about what the specification tries to tell.

I am currently reading the [Chapter 2: The Structure of the Java Virtual Machine](https://docs.oracle.com/javase/specs/jvms/se10/html/jvms-2.html). Here is what I got to learn.

## Java Virtual Machine Specification
The first line states

> This document specifies an abstract machine. It does not describe any particular implementation of the Java Virtual Machine.

This means that the entire specification is an abstract and anyone could implement their own Java Virtual Machine in their preferred programming language. I also googled to see for an already existing implementations of the JVM and found [this list](https://en.wikipedia.org/wiki/List_of_Java_virtual_machines).

I think the most widely used JVM implementation is the [Hotspot](https://en.wikipedia.org/wiki/HotSpot), which is distributed and maintained by Oracle. Just to show the interesting flavours that are possible, here is an implementation of JVM written in Go - https://github.com/zxh0/jvm.go (Thought of doing this when I learnt that JVM is an abstract machine and could be implemented on our own. But was surprised to discover this.)

The specification also tells that it does not mandate few details and leave them upto the choice of the implementors, like

- Garbage collection algorithm used in the JVM.
- Memory layout of run-time data areas (not really sure of what this is)
- Any internal optimization of the Java Virtual Machine instructions

Now, that we know some scope of what to expect, I moving forward.

## class file format

The `class` file format seems to be pretty big deal.

> Compiled code to be executed by the Java Virtual Machine is represented using a hardware- and operating system-independent binary format, typically (but not necessarily) stored in a file, known as the class file format. The class file format precisely defines the representation of a class or interface, including details such as byte ordering that might be taken for granted in a platform-specific object file format.

I used to do Java long time back (mostly in college) and I vividly remember using `javac` and `java`. The `javac` stands for java compiler. We input a java source code file into javac compiler and it produces a `.class` file, which we give as input to `java` program, which will then run our code. It is good that I remember this much.

```
$ man javac

NAME
       javac - Java compiler
```

```
$ man java

NAME
       java - Java application launcher
```

At this point, I am curious to note the contents of a simple `.class` file. So, let me write a Hello world program in java and use `javac` to compile the source to a `.class` file.

```java
// Hello.java

class Hello {
  public static void main(String[] args) {
    System.out.println("Hello world");
  }
}
```

After doing `javac Hello.java`, I got a file named `Hello.class` in the same directory. The contents of the file are

```
����   5 
  	   
     <init> ()V Code LineNumberTable main ([Ljava/lang/String;)V 
SourceFile 
Hello.java      Hello world    Hello java/lang/Object java/lang/System out Ljava/io/PrintStream; java/io/PrintStream println (Ljava/lang/String;)V               	        *� �    
        	    	   %     	� � �    
   
        
   
```

Well, I am pretty intrigued at this point if I could make some sense out the of class file by writing more simple programs and comparing the class file contents.

```java
// Calculate.java

class Calculate {
  public int add(int a, int b) {
    return a + b;
  }

  public int subtract(int a, int b) {
    return a - b;
  }

  public int multiply(int a, int b) {
    return a * b;
  }

  public int divide(int a, int b) {
    return a / b;
  }

  public static void main(String[] args) {
    Calculate c = new Calculate();

    System.out.println(c.add(1,2));
    System.out.println(c.subtract(2,1));
    System.out.println(c.multiply(1,2));
    System.out.println(c.divide(2,1));
  }
}
```
`Calculate.class` contains the following content.
```
����   5 )
 
  
  	  
  
  
  
  
  ! " <init> ()V Code LineNumberTable add (II)I subtract multiply divide main ([Ljava/lang/String;)V 
SourceFile Calculate.java   	Calculate # $ %   & ' (       java/lang/Object java/lang/System out Ljava/io/PrintStream; java/io/PrintStream println (I)V    
           
        *� �                
        `�                
        d�                
        h�                
        l�            	    
   e     9� Y� L� +� � � +� � � +� � � +� 	� �                   ,  8       
```

I could see the presence of our member methods of the class and some letters that my editor could not render. My guess is that they are instruction codes and they would refer to the methods being called. Anyway, we will get to know about this better in the upcoming chapters. So, let me move on.

Here are some pictures from the [clojure getting started guide](https://clojure.org/guides/learn/syntax#_evaluation), that potrays what I just did.

#### Java Evaluation
![Java Evaluation](https://clojure.org/images/content/guides/learn/syntax/traditional-evaluation.png)

#### Clojure Evaluation
![Clojure Evaluation](https://clojure.org/images/content/guides/learn/syntax/clojure-evaluation.png)

Also it is worth noting this statement that is present along with these pictures.

> In Java, source code (.java files) are read as characters by the compiler (javac), which produces bytecode (.class files) which can be loaded by the JVM.

It refers to what I called `.class` file all throughout my little expedition as _bytecode_. OMG! So this is just it. `Bytecode` is not a buzz word to me anymore. I think, the weird characters that my editor could not render are just bytes of data, that represent the code I wrote. Pretty good theory, huh?

Well. Lets see where this goes!
