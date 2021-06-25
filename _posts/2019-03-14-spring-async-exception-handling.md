---
layout: post
title: Spring @Async 的异常处理
categories: 技术
tags: ["Spring", "@Async"]
---

* 目录
{:toc .toc}
楼主在前面的2篇文章中，分别介绍了 Java 子线程中通用的异常处理，以及 Spring Web 应用中的异常处理。

链接如下:

[Java子线程中的异常处理（通用）](http://www.cnblogs.com/yangfanexp/p/7594557.html)

[Spring Web引用中的异常处理](http://www.cnblogs.com/yangfanexp/p/7616570.html)

今天，要写的是被 Spring @Async 注解的方法中的异常处理方法。

通常，如果我们要在程序中做一个耗时的操作（例如调用其他外部模块），一般会通过异步的方式执行。

有这两种方法：

* 自行生成线程池 ThreadPoolExecutor，提交任务执行。
* 更方便地，使用 Spring @Async 注解，修饰在需要异步执行的方法上。

对于第一种方法的异常处理，楼主已经在 “Java 子线程中的异常处理（通用）”这篇文章中介绍了，也就是提交任务后获取到 Future 对象，通过 future.get() 获取返回值的时候能够捕获到 ExcecutionException。

对于 Spring @Async 注解的方法，如何进行异常处理呢？

楼主想到了2种方法。

## 解决办法

### 方法一：配置 AsyncUncaughtExceptionHandler（对于无返回值的方法）

通过 AsyncConfigurer 自定义线程池，以及异常处理。

```java
@Configuration
@EnableAsync
public class SpringAsyncConfiguration implements AsyncConfigurer {
    
    private static final Logger logger = LoggerFactory.getLogger(getClass());
    
    @Bean
    @Override
    public Executor getAsyncExecutor() {
        ThreadPoolTaskExecutor executor = new ThreadPoolTaskExecutor();
        executor.setCorePoolSize(8);
        executor.setMaxPoolSize(16);
        executor.setQueueCapacity(64);
        executor.setRejectedExecutionHandler(new ThreadPoolExecutor.CallerRunsPolicy());
        executor.setThreadNamePrefix("SpringAsyncThread-");
        return executor;
    }

    @Override
    public AsyncUncaughtExceptionHandler getAsyncUncaughtExceptionHandler() {
        return new SpringAsyncExceptionHandler();
    }

    class SpringAsyncExceptionHandler implements AsyncUncaughtExceptionHandler {
        @Override
        public void handleUncaughtException(Throwable throwable, Method method, Object... obj) {
            logger.error("Exception occurs in async method", throwable.getMessage());
        }
    }

}
```

### 方法二：通过 AsyncResult 捕获异常（对于有返回值的方法）

如果异步方法有返回值，那就应当返回 AsyncResult 类的对象，以便在调用处捕获异常。

因为 AsyncResult 是 Future 接口的子类，所以也可以通过 future.get() 获取返回值的时候捕获ExcecutionException。

异步方法：

```java
@Service
public class AsyncService {

    @Async
    public AsyncResult<String> asyncMethodWithResult() {
        // do something（可能发生异常）
        return new AsyncResult("hello");
    }

}
```

调用处捕获异常：

```java
public class Test {

    private Logger logger = LoggerFactory.getLogger(getClass());

    @Autowired
    AsyncService asyncService;

    public void test() {
        try {
            Future future = asyncService.asyncMethodWithResult();
            future.get();
        } catch (ExecutionException e) {
            logger.error("exception occurs", e);
        } catch (InterruptedException e) {
            logger.error("exception occurs", e);
        }
    }

}
```

---

原文：

[Spring @Async 的异常处理](https://www.cnblogs.com/yangfanexp/p/7747225.html)