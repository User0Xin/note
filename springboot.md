## JdbcTemplate

* xxxTemplate 就是springboot配置好的一些bean，拿来即用

## springboot data

* springboot原生使用的是hikari数据源，可以通过配置将其改为其他的数据源如druid
* druid的配置

```java
/**
 * druid的配置
 */

@Configuration
public class DruidConfig {

    //使用yaml对bean进行注入，将其与yaml绑定起来
    @ConfigurationProperties(prefix = "spring.datasource")
    @Bean
    public DataSource druidDataSource(){
        return new DruidDataSource();
    }

    //druid的后台监控 由于springboot把servlet内置了，没有web.xml, 所以需要在这里配置
    @Bean
    public ServletRegistrationBean StatViewServlet(){
        ServletRegistrationBean<StatViewServlet> bean = new ServletRegistrationBean<>(new StatViewServlet(), "/druid/*");
        //后台需要有人登录
        HashMap<String, String> initParameters = new HashMap<>();
        //---------------配置的代码（其他地方都是固定的）-----------------
        initParameters.put("loginUsername","admin");   //loginUsername、loginPassword这两个key是固定的不能改
        initParameters.put("loginPassword","123456");

        //允许谁访问
        initParameters.put("allow","");

        //禁止谁访问
        //initParameters.put("you","192.168.0.1");


        //----------------------------------------
        bean.setInitParameters(initParameters); //设置初始化参数
        return bean;
    }

    //filter
    @Bean
    public FilterRegistrationBean filterRegistrationBean(){
        FilterRegistrationBean bean = new FilterRegistrationBean();
        bean.setFilter(new WebStatFilter());
        HashMap<String, String> initParameters = new HashMap<>();
        //---------------配置的代码（其他地方都是固定的）-----------------

        //这些东西不进行统计
        initParameters.put("exclusions","*.js,*.css,/druid/*");

        
        //----------------------------------------
        bean.setInitParameters(initParameters);
        return bean;
    }
}
```



## springboot整合mybatis

1. 导入包
```xml
<dependency>
    <groupId>org.mybatis.spring.boot</groupId>
    <artifactId>mybatis-spring-boot-starter</artifactId>
    <version>2.1.1</version>
</dependency>
```
2. 配置mybatis
```yaml
spring:
  datasource:
    username: root
    password: youxin
    url: jdbc:mysql://localhost:3306/mybatis_db?serverTimezone=GMT&useUnicode=true&characterEncoding=UTF-8
    driver-class-name: com.mysql.cj.jdbc.Driver


#整合Mybatis
mybatis:
  type-aliases-package: com.springbootlearn.springboot06mybatis.pojo
  mapper-locations: classpath:mybatis/mapper/*.xml
```
3. 编写Mybatis的mapper等，使用`@Mapper`标注该接口是Mybatis的Mapper
```java

@Mapper    //关键注解，标志这个类是Mybatis的Mapper
@Repository    //将这个类放入spring容器中，@Repository 代表了dao层
public interface studentMapper {
    //获取所有学生
    List<student> getStudent();
    //根据id删除学生
    void deleteStudentById(@Param("id")int id);

}
```
4. 把Mybatis的Mapper的xml文件放在Resource目录下，新建一个mybatis目录（classpath:mybatis/mapper/*.xml）
![img.png](img.png)











# 任务

## 异步任务

- 调用一个要处理很久的任务，如果不使用异步处理，前端会卡住等处理完再跳转为了优化体验，将这个设为异步任务，在service的对应方法上面加上@Async注解，同时在启动类上加上@EnableAsync注解表示开启异步处理

```java
@Service
public class AsyncService {
    @Async
    public void asyncwork(){
        try {
            Thread.sleep(3000);
        } catch (InterruptedException e) {
            throw new RuntimeException(e);
        }
        System.out.println("处理完成");
    }
}
```

```java
@GetMapping("/AcyncServ")
public String AcyncServ(){
    //调用异步任务，asyncwork这个任务要处理很久，如果不使用异步处理，前端会卡住等处理完再跳转
    //为了优化体验，将asyncwork设为异步任务，在AsyncService的对应方法上面加上@Async注解
    //同时在启动类上加上@EnableAsync注解表示开启异步处理
    asyncService.asyncwork();
    return "OK";
}
```

```java
@SpringBootApplication
@EnableAsync
public class SpringbootTaskApplication {

    public static void main(String[] args) {
        SpringApplication.run(SpringbootTaskApplication.class, args);
    }

}
```



## 邮件任务

- 邮件发送，在我们的日常开发中，也非常的多，Springboot也帮我们做了支持

- 邮件发送需要引入spring-boot-start-mail

- SpringBoot 自动配置MailSenderAutoConfiguration

- 定义MailProperties内容，配置在application.yml中

- 自动装配JavaMailSender

- 测试邮件发送

演示

### **引入pom依赖**

```java
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-mail</artifactId>
</dependency>
```

### **查看自动配置类：MailSenderAutoConfiguration**

![image-20231002180449463](C:\Users\admin\Desktop\学习\笔记\springboot.assets\image-20231002180449463.png)

   这个类中存在bean，JavaMailSenderImpl

![image-20231002180501456](C:\Users\admin\Desktop\学习\笔记\springboot.assets\image-20231002180501456.png)

  然后我们去看下配置文件

![image-20231002180513916](C:\Users\admin\Desktop\学习\笔记\springboot.assets\image-20231002180513916.png)

### **配置文件：**

```properties
spring.mail.username=24736743@qq.com
spring.mail.password=yhkrgtqwbnrcbhcj
spring.mail.host=smtp.qq.com
# qq需要配置ssl
spring.mail.properties.mail.smtp.ssl.enable=true
```

###  **Spring单元测试**

```java
@Autowired
JavaMailSenderImpl mailSender;
@Test
public void contextLoads() {
//邮件设置1：一个简单的邮件
SimpleMailMessage message = new SimpleMailMessage();
    message.setSubject("通知-明天来狂神这听课");
    message.setText("今晚7:30开会");
    message.setTo("24736743@qq.com");
    message.setFrom("24736743@qq.com");
    mailSender.send(message);
}
@Test
public void contextLoads2() throws MessagingException {
    //邮件设置2：一个复杂的邮件
    MimeMessage mimeMessage = mailSender.createMimeMessage();
    MimeMessageHelper helper = new MimeMessageHelper(mimeMessage, true);
    helper.setSubject("通知-明天来狂神这听课");
    helper.setText("<b style='color:red'>今天 7:30来开会</b>",true);
    //发送附件
    helper.addAttachment("1.jpg",new File(""));
    helper.addAttachment("2.jpg",new File(""));
    helper.setTo("24736743@qq.com");
    helper.setFrom("24736743@qq.com");
    mailSender.send(mimeMessage);
}
```



## 定时任务

项目开发中经常需要执行一些定时任务，比如需要在每天凌晨的时候，分析一次前一天的日志信息，

Spring为我们提供了异步执行任务调度的方式，提供了两个接口。

- TaskExecutor接口

- TaskScheduler接口

两个注解：

- @EnableScheduling

- @Scheduled

![image-20231002204439453](C:\Users\admin\Desktop\学习\笔记\springboot.assets\image-20231002204439453.png)

###  创建一个ScheduledService

我们里面存在一个hello方法，他需要定时执行，怎么处理呢？

```java
@Service
public class ScheduledService {
    //秒 分 时 日 月 周几
    //0 * * * * MON-FRI
    //注意cron表达式的用法；
    @Scheduled(cron = "0 * * * * 0-7")
        public void hello(){
        System.out.println("hello.....");
    }
}
```



###  这里写完定时任务之后，我们需要在主程序上增加@EnableScheduling 开启定时任务功能

```java
@EnableAsync //开启异步注解功能
@EnableScheduling //开启基于注解的定时任务
@SpringBootApplication
public class SpringbootTaskApplication {
    public static void main(String[] args) {
        SpringApplication.run(SpringbootTaskApplication.class, args);
    }
}
```

详细了解下cron表达式:

http://www.bejson.com/othertools/cron/



### 课堂练习

```java
/*
    【0 0/5 14,18 * * ?】每天14点整和18点整，每隔5分钟执行一次
    【0 15 10 ？ * 1-6】每个月的周一-周六10:15分执行一次
    【0 0 2 ？ * 6L】每个月的最后一个周六凌晨2点执行一次
    【0 0 2 LW * ？】每个月的最后一个工作日凌晨2点执行一次
    【0 0 2-4 ？ * 1#1】每个月的第一个周一凌晨2点到4点期间，每个整点都执行一次
*/
```







# @Transient

- 添加在实体类里面的注解，表示该属性不会在数据库当中只是作为一个额外的属性便于操作

![image-20231101131617818](C:\Users\admin\Desktop\学习\笔记\springboot.assets\image-20231101131617818.png)



# 代码健壮性

- 在进行比较时使用一个不可能为空的字段去.equals这样就不会出现空指针异常，如下面所示：如果使用comment.getPid().equals()这里pid可能为空这样就会出现空指针异常，但是使用rootComment.getId().equals()因为主键不可能为空所以这里就不会出现空指针异常了

![image-20231101131845532](C:\Users\admin\Desktop\学习\笔记\springboot.assets\image-20231101131845532.png)





# 配置mybatis开启驼峰命名

- 这样在数据库中的下划线风格的列名就可以映射到java中的驼峰命名

![image-20231101190249247](C:\Users\admin\Desktop\学习\笔记\springboot.assets\image-20231101190249247.png)





# 解决跨域问题

```java
package com.userxin.managesystem.config;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.cors.CorsConfiguration;
import org.springframework.web.cors.UrlBasedCorsConfigurationSource;
import org.springframework.web.filter.CorsFilter;

/**
 * @ClassName: CorsConfig
 * @Description: 解决跨域问题配置
 * @Version 1.0
 * @Date: 2023-11-17 19:50
 * @Auther: UserXin
 */
@Configuration
public class CorsConfig {
    // 当前跨域请求最大有效时长。这里默认1天
    private static final long MAX_AGE = 24 * 60 * 60;

    @Bean
    public CorsFilter corsFilter() {
        UrlBasedCorsConfigurationSource source = new UrlBasedCorsConfigurationSource();
        CorsConfiguration corsConfiguration = new CorsConfiguration();
        corsConfiguration.addAllowedOrigin("*"); // 1 设置访问源地址
        corsConfiguration.addAllowedHeader("*"); // 2 设置访问源请求头
        corsConfiguration.addAllowedMethod("*"); // 3 设置访问源请求方法
        corsConfiguration.setMaxAge(MAX_AGE);
        source.registerCorsConfiguration("/**", corsConfiguration); // 4 对接口配置跨域设置
        return new CorsFilter(source);
    }
}

```



# 集成swagger

## 导入依赖

```xml
 <!--swagger-->
        <dependency>
            <groupId>io.springfox</groupId>
            <artifactId>springfox-swagger2</artifactId>
            <version>3.0.0</version>
        </dependency>

        <dependency>
            <groupId>io.springfox</groupId>
            <artifactId>springfox-boot-starter</artifactId>
            <version>3.0.0</version>
        </dependency>


        <!-- https://mvnrepository.com/artifact/io.springfox/springfox-swagger-ui -->
        <dependency>
            <groupId>io.springfox</groupId>
            <artifactId>springfox-swagger-ui</artifactId>
            <version>3.0.0</version>
        </dependency>
```

## 配置类

```java
package com.userxin.managesystem.config;
import org.springframework.boot.actuate.autoconfigure.endpoint.web.CorsEndpointProperties;
import org.springframework.boot.actuate.autoconfigure.endpoint.web.WebEndpointProperties;
import org.springframework.boot.actuate.autoconfigure.web.server.ManagementPortType;
import org.springframework.boot.actuate.endpoint.ExposableEndpoint;
import org.springframework.boot.actuate.endpoint.web.*;
import org.springframework.boot.actuate.endpoint.web.annotation.ControllerEndpointsSupplier;
import org.springframework.boot.actuate.endpoint.web.annotation.ServletEndpointsSupplier;
import org.springframework.boot.actuate.endpoint.web.servlet.WebMvcEndpointHandlerMapping;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.core.env.Environment;
import org.springframework.util.StringUtils;
import org.springframework.web.servlet.config.annotation.EnableWebMvc;
import org.springframework.web.servlet.config.annotation.ResourceHandlerRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;
import org.springframework.web.servlet.view.InternalResourceViewResolver;
import springfox.documentation.builders.ApiInfoBuilder;
import springfox.documentation.builders.PathSelectors;
import springfox.documentation.builders.RequestHandlerSelectors;
import springfox.documentation.service.ApiInfo;
import springfox.documentation.service.Contact;
import springfox.documentation.spi.DocumentationType;
import springfox.documentation.spring.web.plugins.Docket;
import springfox.documentation.swagger2.annotations.EnableSwagger2;

import java.util.ArrayList;
import java.util.Collection;
import java.util.List;

@EnableSwagger2
@Configuration
@EnableWebMvc
public class SwaggerConfig implements WebMvcConfigurer {
    @Override
    public void addResourceHandlers(ResourceHandlerRegistry registry) {
        // 解决静态资源⽆法访问
        registry.addResourceHandler("/**")
                .addResourceLocations("classpath:/static/");

        registry.addResourceHandler("swagger-ui.html")
                .addResourceLocations("classpath:/META-INF/resources/");

        registry.addResourceHandler("/webjars/**")
                .addResourceLocations("classpath:/META-INF/resources/webjars/");
    }


    @Bean
    public Docket commonDocket() {
        return new Docket(DocumentationType.SWAGGER_2)
                .groupName("后台管理系统")
                .apiInfo(apiInfo("后台管理系统相关接口文档"))
                .pathMapping("/")
                .select()
                .apis(RequestHandlerSelectors.basePackage("com.userxin.managesystem.controller"))
                .paths(PathSelectors.any())
                .build();
    }

    private ApiInfo apiInfo(String desc) {
        return new ApiInfoBuilder()
                .title(desc)
                .version("0.0.1")
                .contact(new Contact("User0Xin", "https://github.com/", "786837227@qq.com"))
                .description("ManageSystem后台相关接口文档")
                .build();

    }

    @Bean
    public WebMvcEndpointHandlerMapping webEndpointServletHandlerMapping(WebEndpointsSupplier webEndpointsSupplier,
                                                                         ServletEndpointsSupplier servletEndpointsSupplier, ControllerEndpointsSupplier controllerEndpointsSupplier,
                                                                         EndpointMediaTypes endpointMediaTypes, CorsEndpointProperties corsProperties,
                                                                         WebEndpointProperties webEndpointProperties, Environment environment) {
        List<ExposableEndpoint<?>> allEndpoints = new ArrayList();
        Collection<ExposableWebEndpoint> webEndpoints = webEndpointsSupplier.getEndpoints();
        allEndpoints.addAll(webEndpoints);
        allEndpoints.addAll(servletEndpointsSupplier.getEndpoints());
        allEndpoints.addAll(controllerEndpointsSupplier.getEndpoints());
        String basePath = webEndpointProperties.getBasePath();
        EndpointMapping endpointMapping = new EndpointMapping(basePath);
        boolean shouldRegisterLinksMapping = this.shouldRegisterLinksMapping(webEndpointProperties, environment,
                basePath);
        return new WebMvcEndpointHandlerMapping(endpointMapping, webEndpoints, endpointMediaTypes,
                corsProperties.toCorsConfiguration(), new EndpointLinksResolver(allEndpoints, basePath),
                shouldRegisterLinksMapping, null);
    }

    private boolean shouldRegisterLinksMapping(WebEndpointProperties webEndpointProperties, Environment environment,
                                               String basePath) {
        return webEndpointProperties.getDiscovery().isEnabled() && (StringUtils.hasText(basePath)
                || ManagementPortType.get(environment).equals(ManagementPortType.DIFFERENT));
    }

    @Bean
    public InternalResourceViewResolver viewResolver() {
        return new InternalResourceViewResolver();
    }
}
```



## 开启api

```java
@SpringBootApplication
@EnableOpenApi
public class ManageSystemApplication {

    public static void main(String[] args) {
        SpringApplication.run(ManageSystemApplication.class, args);
    }

}
```



# PageHelper分页

## 导入依赖

```xml
<dependency>
    <groupId>com.github.pagehelper</groupId>
    <artifactId>pagehelper</artifactId>
    <version>5.3.2</version>
</dependency>
<dependency>
    <groupId>com.github.pagehelper</groupId>
    <artifactId>pagehelper-spring-boot-starter</artifactId>
    <version>1.4.6</version>
</dependency>
```

## service

```java
@Override
    public List<SysUser> selectByNicknameByPage(Integer page, Integer size, String nickName) {
        //创建pageHelper，传入当前页和页面大小
        PageHelper.startPage(page,size); 
        //直接调用mapper的查询方法pageHelper会自动在sql后面加上limit进行分页
        Page<SysUser> sysUsers = sysUserMapper.selectByNicknameByPage(page, size, nickName);
        return sysUsers.getResult();
    }
```







# 部署

## 配置

### 卸载mariadb

```shell
rpm -qa|grep maridb # 查看centos自带的mariadb
rpm -e mariadb-libs-5.5.60-1.el7_5.x86_64 --nodeps #卸载自带的mariadb
```

### 安装mysql

```shell
tar xvf mysql-5.7.44-1.el7.x86_64.rpm-bundle.tar -C mysql # 解压mysql

#安装依赖
yum -y install libaio
yum -y install libncurses*
yum -y install perl-devel

#安装mysql
rpm -ivh mysql-community-common-5.7.44-1.el7.x86_64.rpm
rpm -ivh mysql-community-libs-5.7.44-1.el7.x86_64.rpm
rpm -ivh mysql-community-client-5.7.44-1.el7.x86_64.rpm
rpm -ivh mysql-community-server-5.7.44-1.el7.x86_64.rpm
```

### 启动mysql

```shell
systemctl start mysqld.service
#查看临时账号密码
cat /var/log/mysqld.log|grep password

#登录mysql
mysql -u root -p

#降低密码安全检查（仅学习用方便）
set global validate_password_policy=0;
set global validate_password_length=1;
set password = password('123456');

#授予远程连接权限
grant all privileges on *.* to 'root' @'%' identified by 'root';
#刷新
flush privileges;

#设置开机自启
systemctl enable mysqld
#查看是否设置成功
systemctl  list-unit-files | grep mysqld

#关闭防火墙
firewall-cmd --state #查看防火墙状态
systemctl stop firewalld.service #停止防火墙
systemctl disable firewalld.service #禁止防火墙开机自启
```



### 安装nginx

```shell
# yum安装
yum install epel-release 
yum update #更新yum源
yum -y install nginx #安装
```

### 启动nginx

```shell
systemctl start nginx #启动nginx
systemctl stop nginx #停止nginx
systemctl restart nginx #重启nginx
```



### 配置JDK

```shell
#解压
tar -zvxf jdk-8u391-linux-x64.tar.gz 

#在jdk/bin/etc/profile下加上
vim /etc/profile

export JAVA_HOME=/usr/server/jdk1.8.0_391
export PATH=${JAVA_HOME}/bin:$PATH

#使用配置
source /etc/profile
```



## 部署vue

```shell
#在vue项目下运行打包，会打包出来一个dist目录，将其放到服务器中
npm run build
```

### 配置nginx

```shell
#在nginx的etc/nginx/conf.d下新建一个vue.conf(nginx会自动去读取位于conf.d目录下的以conf后缀的配置文件)
#配置如下
server {
        listen  80;
        server_name     localhost;

        location / {
                root /usr/app/dist;
                index index.html;
        }
}

#让配置生效
nginx -s reload

```



## 打包java程序

- 在idea中使用maven的package工具进行打包，在target中就会生成一个打包好的jar包

![image-20231121171810515](C:\Users\admin\Desktop\学习\笔记\springboot.assets\image-20231121171810515.png)



```shell
#执行java代码 nohup（后台执行）
nohup java -jar k8sManagement-0.0.1-SNAPSHOT.jar > logName.log 2>&1 &

```





# 登录校验

## 导入jwt依赖

```xml
<dependency>
    <groupId>io.jsonwebtoken</groupId>
    <artifactId>jjwt</artifactId>
    <version>0.9.1</version>
</dependency>
 <dependency>
     <groupId>org.springframework.boot</groupId>
     <artifactId>spring-boot-configuration-processor</artifactId>
     <optional>true</optional>
</dependency>
```



## 创建jwt工具类

```java
package com.userxin.managesystem.utils;

import io.jsonwebtoken.Claims;
import io.jsonwebtoken.JwtBuilder;
import io.jsonwebtoken.Jwts;
import io.jsonwebtoken.SignatureAlgorithm;

import java.nio.charset.StandardCharsets;
import java.util.Date;
import java.util.Map;

/**
 * @ClassName: JwtUtil
 * @Description: Jwt生成工具
 * @Version 1.0
 * @Date: 2023-11-25 13:41
 * @Auther: UserXin
 */
public class JwtUtil {
    /**
     * 生成jwt
     * 使用Hs256算法, 私匙使用固定秘钥
     *
     * @param secretKey jwt秘钥
     * @param ttlMillis jwt过期时间(毫秒)
     * @param claims    设置的信息
     * @return
     */
    public static String createJWT(String secretKey, long ttlMillis, Map<String, Object> claims) {
        // 指定签名的时候使用的签名算法，也就是header那部分
        SignatureAlgorithm signatureAlgorithm = SignatureAlgorithm.HS256;

        // 生成JWT的时间
        long expMillis = System.currentTimeMillis() + ttlMillis;
        Date exp = new Date(expMillis);

        // 设置jwt的body
        JwtBuilder builder = Jwts.builder()
                // 如果有私有声明，一定要先设置这个自己创建的私有的声明，这个是给builder的claim赋值，一旦写在标准的声明赋值之后，就是覆盖了那些标准的声明的
                .setClaims(claims)
                // 设置签名使用的签名算法和签名使用的秘钥
                .signWith(signatureAlgorithm, secretKey.getBytes(StandardCharsets.UTF_8))
                // 设置过期时间
                .setExpiration(exp);

        return builder.compact();
    }

    /**
     * Token解密
     *
     * @param secretKey jwt秘钥 此秘钥一定要保留好在服务端, 不能暴露出去, 否则sign就可以被伪造, 如果对接多个客户端建议改造成多个
     * @param token     加密后的token
     * @return
     */
    public static Claims parseJWT(String secretKey, String token) {
        // 得到DefaultJwtParser
        Claims claims = Jwts.parser()
                // 设置签名的秘钥
                .setSigningKey(secretKey.getBytes(StandardCharsets.UTF_8))
                // 设置需要解析的jwt
                .parseClaimsJws(token).getBody();
        return claims;
    }

}
```



## 编辑配置类(从yaml文件中读取进行装配)

```java
package com.userxin.managesystem.properties;

import lombok.Data;
import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.stereotype.Component;

/**
 * @ClassName: JwtProperties
 * @Description: Jwt配置
 * @Version 1.0
 * @Date: 2023-11-25 13:54
 * @Auther: UserXin
 */
@Component
@ConfigurationProperties(prefix = "xin.jwt")
@Data
public class JwtProperties {
    private String secretKey;   //设置jwt签名加密时使用的秘钥
    private long ttl;  //jwt过期时间
    private String tokenName;  //设置前端传递过来的令牌名称
}

```



## 编辑yml文件

```yaml
xin:
  jwt:
    secret-key: UserXin
    ttl: 7200000
    token-name: token
```



## service实现

```java
 @Override
    public SysUserVo checkLogin(String userName, String password) {
        SysUser sysUser = sysUserMapper.selectByUserName(userName);
        if(password.equals(sysUser.getPassword())){
            //存入令牌的信息
            Map<String,Object> claims = new HashMap<>();
            claims.put("id",sysUser.getId());
            claims.put("userName",sysUser.getUsername());
            //生成jwt令牌
            String token = JwtUtil.createJWT(jwtProperties.getSecretKey(), jwtProperties.getTtl(), claims);
            SysUserVo sysUserVo = new SysUserVo();
            sysUserVo.setNickname(sysUser.getNickname());
            sysUserVo.setToken(token);
            return sysUserVo;
        }else{
            return null;
        }
    }
```



- 返回token给前端保存，后面前端的每次请求都要带上token



## 拦截器

```java
package com.userxin.managesystem.interceptor;

import com.userxin.managesystem.context.BaseContext;
import com.userxin.managesystem.properties.JwtProperties;
import com.userxin.managesystem.utils.JwtUtil;
import io.jsonwebtoken.Claims;
import lombok.extern.slf4j.Slf4j;
import org.springframework.stereotype.Component;
import org.springframework.web.method.HandlerMethod;
import org.springframework.web.servlet.HandlerInterceptor;

import javax.annotation.Resource;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * @ClassName: JwtTokenInterceptor
 * @Description: Jwt校验拦截器
 * @Version 1.0
 * @Date: 2023-11-25 15:23
 * @Auther: UserXin
 */
@Component
@Slf4j
public class JwtTokenInterceptor implements HandlerInterceptor {

    @Resource
    private JwtProperties jwtProperties;

    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        //判断当前拦截到的是Controller的方法还是其他资源
        if (!(handler instanceof HandlerMethod)) {
            //当前拦截到的不是动态方法，直接放行
            return true;
        }

        //1、从请求头中获取令牌
        String token = request.getHeader(jwtProperties.getTokenName());

        //2、校验令牌
        try {
            Claims claims = JwtUtil.parseJWT(jwtProperties.getSecretKey(), token);
            //这里可以从claims中取出之前存入jwt的信息，放在threadLocal中进行使用
            Integer id = (Integer) claims.get("id");
            BaseContext.setCurrentId(id);
            //3、通过，放行
            return true;
        } catch (Exception ex) {
            //4、不通过，响应401状态码
            response.setStatus(401);
            return false;
        }


    }
}

```

## 将自定义的拦截器注册到Springboot中

```java
package com.userxin.managesystem.config;

import com.userxin.managesystem.interceptor.JwtTokenInterceptor;
import lombok.extern.slf4j.Slf4j;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.*;

import javax.annotation.Resource;

/**
 * @ClassName: WebMvcConfig
 * @Description: WebMvc配置类
 * @Version 1.0
 * @Date: 2023-11-18 10:00
 * @Auther: UserXin
 */

@Configuration
@Slf4j
public class WebMvcConfig  implements WebMvcConfigurer {

    @Resource
    private JwtTokenInterceptor jwtTokenInterceptor;

    //注册自定义拦截器
    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        log.info("开始注册自定义拦截器...");
        //除了登录和注册请求外其他的都进行拦截
        registry.addInterceptor(jwtTokenInterceptor)
                .addPathPatterns("/**")
                .excludePathPatterns("/Login/**","/Sign/**")
                .excludePathPatterns("/swagger-resources/**", "/webjars/**", "/v2/**", "/swagger-ui.html/**");
    }

}

```



## ThreadLocal实现

```java
package com.userxin.managesystem.context;

/**
 * @ClassName: BaseContext
 * @Description: 用于存储ThreadLocal
 * @Version 1.0
 * @Date: 2023-11-25 15:28
 * @Auther: UserXin
 */
public class BaseContext {

    public static ThreadLocal<Integer> threadLocal = new ThreadLocal<>();

    public static void setCurrentId(Integer id) {
        threadLocal.set(id);
    }

    public static Integer getCurrentId() {
        return threadLocal.get();
    }

    public static void removeCurrentId() {
        threadLocal.remove();
    }

}

```



## 在vue中封装统一的请求方法

```vue
import axios from 'axios'
import router from "@/router";
import {serverIp} from "../../public/config";

const request = axios.create({
    // baseURL: `http://${serverIp}:8081`,
    baseURL: 'http://localhost:8081',
    timeout: 30000
})

// request 拦截器
// 可以自请求发送前对请求做一些处理
// 比如统一加token，对请求参数统一加密
request.interceptors.request.use(config => {
    config.headers['Content-Type'] = 'application/json;charset=utf-8';
    let user = localStorage.getItem("user") ? JSON.parse(localStorage.getItem("user")) : null
    if (user) {
        config.headers['token'] = user.token;  // 设置请求头
    }
    return config;
}, error => {
    return Promise.reject(error)
});

// response 拦截器
// 可以在接口响应后统一处理结果
request.interceptors.response.use(
    response => {
        let res = response.data;
        // 如果是返回的文件
        if (response.config.responseType === 'blob') {
            return res
        }
        // 兼容服务端返回的字符串数据
        if (typeof res === 'string') {
            res = res ? JSON.parse(res) : res
        }
        // 当权限验证不通过的时候给出提示
        if (res.code === '401') {
            alert("权限不足请登录")
            router.push("/login")
        }
        return res;
    },
    error => {
        console.log('err' + error) // for debug
        if(error.message==="Request failed with status code 401"){
            alert("权限不足请登录");
            router.push("/login");
        }
        return Promise.reject(error)
    }
)


export default request


```

```vue
 //使用封装好的请求方法
        request.post("/Login/" + this.loginForm.userName + "/" + this.loginForm.pass).then(res => {
          console.log(res.data);
          if (res.code === 505) {
            alert(res.msg);
          } else {
            localStorage.setItem('user', JSON.stringify(res.data));
            this.$router.replace("/Manage");
          }
        }).catch(err => {
          console.log(err);
        });
```





​	

# 日期格式转换

## Date转localDateTime

```java
public static LocalDateTime toLocalDateTime(Date date) {
        // 方法一
        LocalDateTime localDateTime = date.toInstant().atZone(ZoneId.systemDefault()).toLocalDateTime();
        // 方法二
        return LocalDateTime.ofInstant(date.toInstant(), ZoneId.systemDefault());
    }
```

## LocalDateTime转Date

```java
  public static Date toDate(LocalDateTime localDateTime) {
        return Date.from(localDateTime.atZone(ZoneId.systemDefault()).toInstant());
    }

```

## 时间字符串转LocalDateTime

```java
/**
     * date string to LocalDateTime
     * @param dateStr eg: "2020-08-22 10:30:50"
     * @param pattern eg: "yyyy-MM-dd HH:mm:ss"
     * @return LocalDateTime
     */
    public static LocalDateTime toLocalDateTime(String dateStr, String pattern) {
        return LocalDateTime.parse(dateStr, DateTimeFormatter.ofPattern(pattern));
    }

```

## LocalDateTime转时间字符串

```java
 public static String localDateTimeToStr(LocalDateTime dateTime, String pattern) {
        if (dateTime == null) {
            return null;
        }

        return dateTime.format(DateTimeFormatter.ofPattern(StrUtil.isBlank(pattern) ? "yyyy-MM-dd HH:mm:ss" : pattern));
    }

```

## Date转OffsetDateTime

```java
public static OffsetDateTime toOffsetDateTime(Date date) {
        // 方法一
        OffsetDateTime offsetDateTime = date.toInstant().atZone(ZoneId.systemDefault()).toOffsetDateTime();
        // 方法二
        return ZonedDateTime.ofInstant(date.toInstant(), ZoneId.systemDefault()).toOffsetDateTime();
    }
    
    // 测试
    public static void test(){
        OffsetDateTime offsetDateTime = toOffsetDateTime(new Date());
        // 当前最后时间 23:59:59
        OffsetDateTime with = offsetDateTime.with(LocalTime.MAX);
        // 当前开始时间 00:00:00
        OffsetDateTime  endDateTime = offsetDateTime.with(LocalTime.MIN);
    }

```

## OffsetDateTime转Date

```java
public static Date toDate(OffsetDateTime offsetDateTime){
        return Date.from(offsetDateTime.atZoneSameInstant(ZoneId.systemDefault()).toInstant());
    }

```



## OffsetDateTime转LocalDateTime

```java
public static LocalDateTime toLocalDateTime(OffsetDateTime offsetDateTime) {
        return LocalDateTime.ofInstant(offsetDateTime.atZoneSameInstant(ZoneId.systemDefault()).toInstant(), ZoneId.systemDefault());
    }

```



## 获取整点时间

```java
 /**
     * 获取整点时间 示例：2020-10-28 10:18:32 返回 2020-10-28 10:00:00
     * @param date the date
     * @return the hour date time
     */
    public static Date getHour(Date date) {
        LocalDateTime dateTime = LocalDateTime.ofInstant(date.toInstant(), ZoneId.systemDefault());
        return Date.from(LocalDateTime.of(dateTime.toLocalDate(), LocalTime.of(dateTime.getHour(), 0, 0)).atZone(ZoneId.systemDefault()).toInstant());
    }
```



## LocalDateTime 转 时间戳（秒级）

```java
@Test
    public void localTimeTest1(){
        // 获得当前时间
        LocalDateTime localDateTime = LocalDateTime.now();
        // 将当前时间转为时间戳
        long second = localDateTime.toEpochSecond(ZoneOffset.ofHours(8));
        System.out.println(second);

    }

@Test
    public void localTimeTest2(){
        // 获得当前时间
        LocalDateTime localDateTime = LocalDateTime.now();
        // 将当前时间转为时间戳
        long second = localDateTime.toInstant(ZoneOffset.ofHours(8)).getEpochSecond();
        System.out.println(second);

    }

 public void localTimeTest3() {
        // 获得当前时间
        LocalDateTime localDateTime = LocalDateTime.now();
        // 将当前时间转为时间戳
        long milliseconds = localDateTime.toInstant(ZoneOffset.ofHours(8)).toEpochMilli();
        System.out.println(milliseconds / 1000);
    }

```



## 时间戳 转LocalDateTime

```java
 public void localTimeTest4() {
        //获得时间戳
        long second = LocalDateTime.now().toInstant(ZoneOffset.of("+8")).getEpochSecond();
        // 将时间戳转为当前时间
        LocalDateTime localDateTime = LocalDateTime.ofEpochSecond(second, 0, ZoneOffset.ofHours(8));
        System.out.println(localDateTime);
    }
public void localTimeTest5() {
        //获得时间戳
        long milliseconds = LocalDateTime.now().toInstant(ZoneOffset.of("+8")).toEpochMilli();
        // 将时间戳转为当前时间
        LocalDateTime localDateTime = LocalDateTime.ofEpochSecond(milliseconds / 1000, 0, ZoneOffset.ofHours(8));
        System.out.println(localDateTime);
    }
 public void localTimeTest6(){
        //获得时间戳
        long milliseconds = LocalDateTime.now().toInstant(ZoneOffset.of("+8")).toEpochMilli();
        // 将时间戳转为当前时间
        LocalDateTime localDateTime = Instant.ofEpochMilli(milliseconds).atZone(ZoneOffset.ofHours(8)).toLocalDateTime();
        System.out.println(localDateTime);
    }


```

## 时间戳与LocalDate互转

### 1.时间戳转LocalDate

```java

    public void localDateTest1(){
        //获得时间戳
        long milliseconds = LocalDateTime.now().toInstant(ZoneOffset.of("+8")).toEpochMilli();
        // 将时间戳转为当前时间
        LocalDate localDate = Instant.ofEpochMilli(milliseconds).atZone(ZoneOffset.ofHours(8)).toLocalDate();
        System.out.println(localDate);
	}
  public void localDateTest2(){
        //获得时间戳
        long seconds = LocalDateTime.now().toInstant(ZoneOffset.of("+8")).getEpochSecond();
        // 将时间戳转为当前时间
        LocalDate localDate = Instant.ofEpochSecond(seconds).atZone(ZoneOffset.ofHours(8)).toLocalDate();
        System.out.println(localDate);
    }

```

### 2.LocalDate 转 时间戳

```java
public void localDateTest3(){
        LocalDate localDate = LocalDate.now();
        //获得时间戳
        long seconds = localDate.atStartOfDay(ZoneOffset.ofHours(8)).toInstant().getEpochSecond();
        System.out.println(seconds);
    }

 public void localDateTest4(){
        LocalDate localDate = LocalDate.now();
        //获得时间戳
        long seconds = localDate.atStartOfDay(ZoneOffset.ofHours(8)).toInstant().toEpochMilli();
        System.out.println(seconds);
    }

```

## 时间格式化输出

```java
@JsonFormat(pattern = "yyyy-MM-dd HH:mm:ss", timezone = "GMT+8")
	private Date time1;
    @JsonFormat(pattern = "yyyy-MM-dd HH:mm:ss", timezone = "GMT+8")
	private LocalDateTime time2;

```



## 备注

1、LocalDateTime可以做日期的加减很方便
2、OffsetDateTime计算00:00:00及23:59:59很方便
