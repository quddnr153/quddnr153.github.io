---
layout: post
title:  "Comparing Spring AOP and AspectJ"
date:   2018-01-02 23:8:0 +0900
tags: web spring aop aspecj java
---
# 1. Introduction
* * *
여러 AOP library 들에 대해서 아래와 같이 다양한 질문들이 있다:
- 기존 혹은 새로운 application 과 호환되나?
- AOP 를 어디서 implement 할 수 있나?
- 어떻게 기존 application과 빠르게 통합할 수 있나?
- 성능 오버헤드는 무엇인가?

지금부터 위 질문에 대한 답변과 Spring AOP, AspectJ 에 대해 알아보자 (Java 에서 가장 인기있는 AOP framework)


* * *
# 2. AOP Concepts
* * *
시작하기에 앞서, terms 와 core concepts 가 무엇인지 빠르게 review 해보자.
- Aspect: Application의 여러 위치에 분산되 있고 실제 비지니스 로직이 아닌 (예를 들면, 트랜잭션 관리) 와 같은 standard code / feature 들
- Joinpoint: method execution, constructor call, 혹은 field assignment 와 같은 프로그램 실행 중의 특정 시점
- Advice: 특정 jointpoint 에서 aspect 가 취해진 action
- Pointcut: jointpoint 와 매치되는 정규 표현식. 매번 어느 jointpoint 와 pointcut 이 매치할 때, 해당 pointcut 과 관련된 advice 가 실행 된다.
- Weaving: advice object 를 생성하기 위해 targeted objects (여러 대상 객체들) 에  aspect 를 linking 하는 과정

* * *
# 4. Spring AOP and AspectJ
* * *
지금부터 Spring AOP 와 AspectJ 를 다양한 측면에서 비교를 해보자 (Capabilities,goals, weaving, internal structure, jointpoints, and simplicity)

## 3.1. Capabilities and Goals

간단하게 말해서, Spring AOP 와 AspectJ 는 다른 goals 를 가진다.

Spring AOP:
- Spring IoC 에서 간단한 AOP 구현을 제공하며 프로그래머가 직면하는 가장 일반적인 문제를 해결하는 것이 목표
- 완전한 AOP solution 으로 의도된 것은 아니며, Spring container 에 의해 관리되는 빈에만 적용될 수 있음

AspectJ:
- 완전한 AOP solution 을 제공하는 것을 목표로하는 original AOP technology
- Spring AOP 보다는 복잡하지만 더 강력한 기능을 제공
- 모든 domain objects 에 적용할 수 있음

## 3.2. Weaving

AspectJ 는 아래 세 가지 유형의 weaving 을 사용한다:
- ***Compile-time weaving***: AspectJ Compiler 는 aspect 와 application 소스 코드를 둘 다 input 으로 간주하고 ouput 으로는 weaving 된 class files 를 제공한다.
- ***Post-compile weaving***: 소위 binary weaving 이라 불린다. 기존 class files 과 jar files 를 weaving 할 때 사용한다. ([see more](http://www.mojohaus.org/aspectj-maven-plugin/examples/weaveJars.html))
- ***Load-time weaving***: binary weaving 과 똑같지만, 클래스 로더가 클래스 파일들을 JVM 에 로드 할 때까지 weaving 을 연기한다는 차이점이 있다.

[see more for AspectJ weaving](http://www.baeldung.com/aspectj)

위에서 봤듯이 AspectJ 는 compile time 과 classload time weaving 을 사용하는 반면,


Spring AOP 는 runtime weaving 을 활용한다:

aspects 는 targeted object 의 프록시를 사용하여 application 실행 중에 weaving 된다.

![Spring AOP process](/assets/img/springaop-process.png)

## 3.3. Internal Structure and Application

Spring AOP is a proxy-based AOP framework. This means that to implement aspects to the target objects, it’ll create proxies of that object. This is achieved using either of two ways:

1. JDK dynamic proxy – the preferred way for Spring AOP. Whenever the targeted object implements even one interface, then JDK dynamic proxy will be used
2. CGLIB proxy – if the target object doesn’t implement an interface, then CGLIB proxy can be used
We can learn more about Spring AOP proxying mechanisms from [the official docs](https://docs.spring.io/spring/docs/current/spring-framework-reference/core.html#aop-proxying).

AspectJ, on the other hand, doesn’t do anything at runtime as the classes are compiled directly with aspects.

And so unlike Spring AOP, it doesn’t require any design patterns. To weave the aspects to the code, it introduces its compiler known as AspectJ compiler (ajc), through which we compile our program and then runs it by supplying a small (< 100K) runtime library.

## 3.4. Joinpoints

In section 3.3, we showed that Spring AOP is based on proxy patterns. Because of this, it needs to subclass the targeted Java class and apply cross-cutting concerns accordingly.

But it comes with a limitation. We cannot apply cross-cutting concerns (or aspects) across classes that are “final” because they cannot be overridden and thus it would result in a runtime exception. 

The same applies for static and final methods. Spring aspects cannot be applied to them because they cannot be overridden. Hence Spring AOP because of these limitations, only supports method execution join points.

However, AspectJ weaves the cross-cutting concerns directly into the actual code before runtime. Unlike Spring AOP, it doesn’t require to subclass the targetted object and thus supports many others joinpoints as well. Following is the summary of supported joinpoints:

|Joinpoint | Spring AOP Supported | AspectJ Supported|
|----|----|----|
|Method Call | No | Yes|
|Method Execution | Yes | Yes|
|Constructor Call | No | Yes|
|Constructor Execution | No | Yes|
|Static initializer execution | No | Yes|
|Object initialization | No | Yes|
|Field reference | No | Yes|
|Field assignment | No | Yes|
|Handler execution | No | Yes|
|Advice execution | No | Yes|

It’s also worth noting that in Spring AOP, aspects aren’t applied to the method called within the same class.

That’s obviously because when we call a method within the same class, then we aren’t calling the method of the proxy that Spring AOP supplies. If we need this functionality, then we do have to define a separate method in different beans, or use AspectJ.

## 3.5. Simplicity

Spring AOP is obviously simpler because it doesn’t introduce any extra compiler or weaver between our build process. It uses runtime weaving, and therefore it integrates seamlessly with our usual build process. Although it looks simple, it only works with beans that are managed by Spring.

However, to use AspectJ, we’re required to introduce the AspectJ compiler (ajc) and re-package all our libraries (unless we switch to post-compile or load-time weaving).

This is, of course, more complicated than the former – because it introduces AspectJ Java Tools (which include a compiler (ajc), a debugger (ajdb), a documentation generator (ajdoc), a program structure browser (ajbrowser)) which we need to integrate with either our IDE or the build tool.

## 3.6. Performance

As far as performance is concerned, compile-time weaving is much faster than runtime weaving. Spring AOP is a proxy-based framework, so there is the creation of proxies at the time of application startup. Also, there are a few more method invocations per aspect, which affects the performance negatively.

On the other hand, AspectJ weaves the aspects into the main code before the application executes and thus there’s no additional runtime overhead, unlike Spring AOP.

For these reasons, the [benchmarks](https://web.archive.org/web/20150520175004/https://docs.codehaus.org/display/AW/AOP+Benchmark) suggest that AspectJ is almost around 8 to 35 times faster than Spring AOP.

# 4. Summary

This quick table summarizes the key differences between Spring AOP and AspectJ:

|Spring AOP | AspectJ|
|---|---|
|Implemented in pure Java | Implemented using extensions of Java programming language|
|No need for separate compilation process | Needs AspectJ compiler (ajc) unless LTW is set up|
|Only runtime weaving is available | Runtime weaving is not available. Supports compile-time, post-compile, and load-time Weaving|
|Less Powerful – only supports method level weaving | More Powerful – can weave fields, methods, constructors, static initializers, final class/methods, etc…|
|Can only be implemented on beans managed by Spring container | Can be implemented on all domain objects|
|Supports only method execution pointcuts | Support all pointcuts|
|Proxies are created of targeted objects, and aspects are applied on these proxies | Aspects are weaved directly into code before application is executed (before runtime)|
|Much slower than AspectJ |Better Performance|
|Easy to learn and apply | Comparatively more complicated than Spring AOP|

# 5. Choosing the Rigth Framework

If we analyze all the arguments made in this section, we’ll start to understand that it’s not at all that one framework is better than another.

Simply put, the choice heavily depends on our requirements:

- Framework: If the application is not using Spring framework, then we have no option but to drop the idea of using Spring AOP because it cannot manage anything that’s outside the reach of spring container. However, if our application is created entirely using Spring framework, then we can use Spring AOP as it’s straightforward to learn and apply
- Flexibility: Given the limited joinpoint support, Spring AOP is not a complete AOP solution, but it solves the most common problems that programmers face. Although if we want to dig deeper and exploit AOP to its maximum capability and want the support from a wide range of available joinpoints, then AspectJ is the choice
- Performance: If we’re using limited aspects, then there are trivial performance differences. But there are sometimes cases when an application has more than tens of thousands of aspects. We would not want to use runtime weaving in such cases so it would be better to opt for AspectJ. AspectJ is known to be 8 to 35 times faster than Spring AOP
- Best of Both: Both of these frameworks are fully compatible with each other. We can always take advantage of Spring AOP whenever possible and still use AspectJ to get support of joinpoints that aren’t supported by the forme

# 6. Conclusion

In this article, we analyzed both Spring AOP and AspectJ, in several key areas.

We compared the two approaches to AOP both on flexibility as well as on how easily they will fit with our application.

## 더 보기

[Spring reference 분석](https://blog.outsider.ne.kr/845)


## Reference
- [Spring reference](https://docs.spring.io/spring/docs/current/spring-framework-reference/core.html#aop-api)
- [Comparing Spring AOP and AspectJ](http://www.baeldung.com/spring-aop-vs-aspectj)
