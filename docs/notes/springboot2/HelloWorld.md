## 每篇一律
```text
云对雨，雪对风，晚照对晴空。
来鸿对去雁，宿鸟对鸣虫。    --《声律启蒙·一东》
```

## 什么是Spring Boot

#### SpringBoot 是为了简化 Spring 应用的创建、运行、调试、部署等一系列问题而诞生的产物，自动装配的特性让我们可以更好的关注业务本身而不是外部的XML配置，我们只需遵循规范，引入相关的依赖就可以分分钟搭建一个WEB

## 构建一个HelloWorld项目

#### 1. 快速生成项目
[Spring官网](https://start.spring.io/)提供了一个工具，我们打开后可以看到这样一个画面

![图片](..//../PhperToJava/_media/SpringBoot2/helloworld.png)


#### 2. 打开项目
下载之后，解压之后看见如下目录，然后使用IDE（推荐使用[idea](https://www.jetbrains.com/idea/download)）打开即可~

![图片](..//../PhperToJava/_media/SpringBoot2/helloworld0.png)


#### 3. 编辑项目
打开项目之后，建立Controller目录，在Controller目录建立HelloController.java文件,写上如下代码，然后点击上方小虫子运行，
```java
@RestController
@RequestMapping(value = "/test")
public class HelloController {

    @GetMapping(value = "/hello")
    public String hello(String name) {
        return "hello" + name;
    }
}
```
效果图如下：
![图片](..//../PhperToJava/_media/SpringBoot2/helloworld1.png)


#### 4. 查看成功
项目默认启动端口是8080，在浏览器输入如下地址访问，可以看见输出的内容
```text
http://127.0.0.1:8080/test/hello?name=bobo
```

## 总结：搭建一个helloworld还是很方便的，摆脱了以前写大量xml的时代
