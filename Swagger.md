# SpringBoot集成Swagger

1. 新建一个SpringBoot项目=>web项目

2. 导入相关依赖

   ```xml
    <!-- https://mvnrepository.com/artifact/io.springfox/springfox-swagger2 -->
           <dependency>
               <groupId>io.springfox</groupId>
               <artifactId>springfox-swagger2</artifactId>
               <version>2.9.2</version>
           </dependency>
   
           <!-- https://mvnrepository.com/artifact/io.springfox/springfox-swagger-ui -->
           <dependency>
               <groupId>io.springfox</groupId>
               <artifactId>springfox-swagger-ui</artifactId>
               <version>2.9.2</version>
           </dependency>
   ```

   

3. 编写一个hello工程

4. 配置swagger==> 写一个Config（在config包下写SwaggerConfig） springboot版本降到2.5.1使用高版本配置有所差异

   ```java
   @Configuration      //表明这是个配置类
   @EnableSwagger2     //开启swagger2
   public class SwaggerConfig {
   }
   ```

   

5. 测试运行 访问swagger-ui.html

   

# 配置Swagger

-  Swagger的bean实例docket,编写自己的apiInfo

```java

@Configuration      //表明这是个配置类
@EnableSwagger2     //开启swagger2
public class SwaggerConfig {
    @Bean
    public Docket docket(){
        return new Docket(DocumentationType.SWAGGER_2).apiInfo(apiInfo());
    }

    private ApiInfo apiInfo(){
        Contact contact =  new Contact("UserXin", "www.UserXin.com", "45645645@qq.com");   //作者信息
        return new ApiInfo(
                "我的SwaggerAPI文档",
                "我们的描述",
                "v1.0",
                "https://localhost:8080",
                contact,  //作者信息
               "Apache 2.0",
                "http://www.apache.org/licenses/LICENSE-2.0",
                new ArrayList());

    }
}

```





## 配置api文档的分组

```xml
return new Docket(DocumentationType.SWAGGER_2).groupName("Xin")
```

配置多个分组就配置多个docket实例就好了

```java
@Bean
public Docket docket01(){
    return new Docket(DocumentationType.SWAGGER_2).groupName("A");
}
@Bean
public Docket docket02(){
    return new Docket(DocumentationType.SWAGGER_2).groupName("B");
}

@Bean
public Docket docket03(){
    return new Docket(DocumentationType.SWAGGER_2).groupName("C");
}
```



## 实体类

```java
@ApiModel("用户类")
public class User {
    @ApiModelProperty("用户名")
    private String name;
    @ApiModelProperty("密码")
    private String password;
}
```



## Controller

```java
@ApiOperation("获取用户名")
@PostMapping("/getName")
public String getName(@ApiParam("用户名") @RequestParam("userName") String username){
    return "hello"+username;
}
```





# ！！！注意点：正式发布时关闭swagger（出于安全考虑同时提高效率）