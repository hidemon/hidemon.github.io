---
layout: post
title: "A simple example for dynamic proxy in Java"
date: 2016-03-27
---

According to the official description of dynammic proxy from [Oracle](https://docs.oracle.com/javase/8/docs/technotes/guides/reflection/proxy.html) :

> A *dynamic proxy class* is a class that implements a list of interfaces specified at runtime such that a method invocation through one of the interfaces on an instance of the class will be encoded and dispatched to another object through a uniform interface. Thus, a dynamic proxy class can be used to create a type-safe proxy object for a list of interfaces without requiring pre-generation of the proxy class, such as with compile-time tools. Method invocations on an instance of a dynamic proxy class are dispatched to a single method in the instance's *invocation handler*, and they are encoded with a `java.lang.reflect.Method` object identifying the method that was invoked and an array of type `Object` containing the arguments.

We can simply generilize this as the following scene:

If you want to buy a house from a owner, you could probabily find a proxy to help you handle all of the trivial stuff. But the proxy itself does not have a house, it just plays the role of a bridge.

Here I give a very direct example to explain the role of proxy in Java.

Firstly, I create two interfaces