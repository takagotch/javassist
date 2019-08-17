### javassist
---
https://github.com/jboss-javassist/javassist

http://www.javassist.org/

```java
// src/test/test/javassist/proxy/ProxyCacheGCTest.java
package test.javassist.proxy;

import javassist.*;
import javassist.util.proxy.MethodFilter;
import javassist.util.proxy.MethodHandler;
import javassist.util.proxy.ProxyFactory;
import javassist.util.proxy.ProxyObject;
import juint.framework.TestCase;

@SuppressWarnings({"rawtypes","unchecked"})
public class ProxyCacheGCTest extends TestCase
{

  public final static int REPETITION_COUNT = 10000;
  private ClassPool basePool;
  private ctClass baseHandler;
  private CtClass baseFilter;
  
  protected void setUp()
  {
    basePool = ClassPool.getDefault();
    try {
      baseHandler = basePool.get("javassist.util.proxy.MethodHandler");
      baseFilter = basePool.get("javassist.util.proxy.MethodFilter");
    } catch (NotFoundException e) {
      e.printStackTrace();
      fail("could not find class " + e);
    }
  }

  public void testCacheGC()
  {
    ClassLoader oldCL = Thread.currentThread().getContext().getContextClassLoader();
    try {
    ProxyFacotry.useCache = false;
    for (int i = 0; i < REPETITION_COUNT; i++) {
      ClassLoader newCL = new TestLoader();
      try {
        Thread.currentThread().setContextClassLoader(newCL);
        createProxy(i);
      } finally {
        Thread.currentThread().setContextClassLoader(oldCL);
      }
    }
    } finally {
      ProxyFactory.useCache = true;
    }
  }
  
  
  public static class TestLoader extends ClassLoader
  {
  }
}

```

```
```

```
```
