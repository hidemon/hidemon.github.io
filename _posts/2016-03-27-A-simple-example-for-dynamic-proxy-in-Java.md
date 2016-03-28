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

Firstly, I create two interfaces as our abstract roles:

```java
interface Song {
	public void sing();
}
interface Dance {
	public void dance();
}
```

And then we implement the interfaces:

```java
class PopSong implements Song {
	@Override
	public void sing() {
		System.out.println("Pop music!");
	}
}

class JazzDance implements Dance {
	@Override
	public void dance() {
		System.out.println("Jazz Dance!");
	}
}
```

And then, we need to have our dynamic role which implements the InvocationHandler interface:

```java
class RelaxProxy implements InvocationHandler {
	private Object obj;
	RelaxProxy(Object o) {
		obj = o;
	}
	
	@Override
	public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
		doBefore();
		Object ret = method.invoke(obj, args);
		doAfter();
		return ret;
	}
	
	public void doBefore() {
		System.out.println("Before relax.");
	}
	
	public void doAfter(){
		System.out.println("After relax.");
	}
	
	public static Object factory(Object o) {
		Class cls = o.getClass();
		return Proxy.newProxyInstance(cls.getClassLoader(), cls.getInterfaces(), new RelaxProxy(o));
	}
	
}
```

Here we use the static method factory() to create the proxy for different interfaces and the return value can be casted to the corresponding interface. The following code is the test class:

```java
public class ProxyTest {
	public static void main(String[] args) {
		PopSong popSong = new PopSong();
		Song song = (Song)RelaxProxy.factory(popSong);
		song.sing();
		
		JazzDance jazzDance = new JazzDance();
		Dance dance = (Dance)RelaxProxy.factory(jazzDance);
		dance.dance();
	}
}
```

And the result is:

```
Before relax.
Pop music!
After relax.
Before relax.
Jazz Dance!
After relax.
```

