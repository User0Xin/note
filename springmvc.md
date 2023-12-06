## springmvc配置：
### web.xml配置
```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">

    <!--    1、注册DispatcherServlet:SpringMVC的核心：请求分发器，前端控制器-->
    <servlet>
        <servlet-name>springmvc</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <!--        关联一个springmvc的配置文件：【servlet-name】-servlet.xml-->
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:springmvc-servlet.xml</param-value>
        </init-param>
        <!--        启动级别：1-->
        <load-on-startup>1</load-on-startup>
    </servlet>


    <!--    /： 匹配所有请求（不包括.jsp）-->
    <!--    /*： 匹配所有请求（包括.jsp）-->

    <servlet-mapping>
        <servlet-name>springmvc</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>

    <!--    springmvc解决乱码问题-->
    <filter>
        <filter-name>encoding</filter-name>
        <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
        <init-param>
            <param-name>encoding</param-name>
            <param-value>utf-8</param-value>
        </init-param>
    </filter>
    <filter-mapping>
        <filter-name>encoding</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>
</web-app>
```
### springmvc的xml配置
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        https://www.springframework.org/schema/context/spring-context.xsd http://www.springframework.org/schema/mvc https://www.springframework.org/schema/mvc/spring-mvc.xsd">

    <!--    自动扫描包，让指定包下的注解生效，交给IOC容器管理-->
    <context:component-scan base-package="com.mySpringMVCLearn.controller"/>
    <!--    让SpringMVC不处理静态资源-->
    <mvc:default-servlet-handler/>
    <!--    支持mvc注解驱动，同时会自动的注册HandlerMapping和HandlerAdapter-->
    <mvc:annotation-driven/>

    <!--    视图解析器：DispacherServlet给他的ModelAndView-->
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver" id="internalResourceViewResolver">
        <!--        前缀-->
        <property name="prefix" value="/WEB-INF/jsp/"/>
        <!--        后缀-->
        <property name="suffix" value=".jsp"/>
    </bean>


</beans>
```

## 注解开发
- @Controller：标记在对应的Controller上，代表该Controller类被Spring管理
- @RequestMapping: 标记在对应的方法上，响应请求
- @PathVariable：在参数上加上@PathVariable注解来实现restFul风格，使得程序可以直接从url上面获取数据，不像原来的需要加上？后面再跟参数
- @GetMapping、@PostMapping等注解代表响应Get、Post请求可以实现url的复用，同一个url使用不同的方法请求就执行不同的方法
- @RequestParam("userName")注解代表从前端接受到的参数名为"userName" ！！！这个注解一定要加，这样可以确保前后端的一致性

### 例子01
```java

package com.mySpringMVCLearn.controller;

import com.mySpringMVCLearn.pojo.User;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.ui.ModelMap;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestParam;

import java.util.Map;

@Controller
public class UserController {
    @RequestMapping("/test")
//    @RequestParam("userName")注解代表从前端接受到的参数名为"userName"
//    ！！！这个注解一定要加，这样可以确保前后端的一致性
    public String test(@RequestParam("userName") String name, Model model){
        System.out.println("从前端接受到的参数"+name);
        model.addAttribute("msg",name);
        return "test";
    }

    /*
    * 接受的是一个对象，只需要保证前端传递的参数与实体类的参数一一对应即可
    * */

    @RequestMapping("/test02")
//    @RequestParam("userName")注解代表从前端接受到的参数名为"userName"
//    ！！！这个注解一定要加，这样可以确保前后端的一致性
    public String test02(User user, Model model){
        System.out.println("从前端接受到的参数"+user);
        model.addAttribute("msg",user);
        return "test";
    }

}

```

### 例子02
```java
package com.mySpringMVCLearn.controller;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.*;

/**
 * 使用restFul风格使得Url更加简洁，且安全
 */

@Controller
public class restFulController{
    @RequestMapping("/test/{a}/{b}")
//    在参数上加上@PathVariable注解来实现restFul风格，使得程序可以直接从url上面获取数据，不像原来的需要加上？后面再跟参数
    public String test(@PathVariable int a,@PathVariable String b, Model model) {
        String res = a+b;
        model.addAttribute("msg","结果为"+res);
        return "test";
    }

//    @GetMapping、@PostMapping等注解代表响应Get、Post请求可以实现url的复用，同一个url使用不同的方法请求就执行不同的方法
    @GetMapping("/test01/{a}/{b}")
    public String test01(@PathVariable int a,@PathVariable String b, Model model) {
        String res = a+b;
        model.addAttribute("msg","结果2为"+res);
        return "test";
    }

}

```