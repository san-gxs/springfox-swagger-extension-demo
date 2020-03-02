# quick start（springboot快速构建）
## 1. maven dependency

    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>   
    <dependency>
        <groupId>com.github.luffytalory</groupId>
        <artifactId>springfox-swagger-extension-core</artifactId>
        <version>1.0.3</version>
    </dependency>
    <dependency>
        <groupId>org.projectlombok</groupId>
        <artifactId>lombok</artifactId>
    </dependency>
    <dependency>
        <groupId>org.apache.dubbo</groupId>
        <artifactId>dubbo-spring-boot-starter</artifactId>
        <version>2.7.3</version>
    </dependency>
    <dependency>
        <groupId>org.apache.dubbo</groupId>
        <artifactId>dubbo</artifactId>
        <version>2.7.3</version>
    </dependency>
    <dependency>
        <groupId>org.apache.dubbo</groupId>
        <artifactId>dubbo-dependencies-zookeeper</artifactId>
        <version>2.7.3</version>
        <exclusions>
            <exclusion>
                <groupId>org.slf4j</groupId>
                <artifactId>slf4j-log4j12</artifactId>
            </exclusion>
        </exclusions>
        <type>pom</type>
    </dependency>
    <!--dubbo rest相关-->
    <dependency>
        <groupId>org.jboss.resteasy</groupId>
        <artifactId>resteasy-jaxrs</artifactId>
        <version>3.9.3.Final</version>
    </dependency>
    <dependency>
        <groupId>org.jboss.resteasy</groupId>
        <artifactId>resteasy-client</artifactId>
        <version>3.9.3.Final</version>
    </dependency>
    <dependency>
        <groupId>org.jboss.resteasy</groupId>
        <artifactId>resteasy-jaxb-provider</artifactId>
        <version>3.9.3.Final</version>
    </dependency>
    <dependency>
        <groupId>org.jboss.resteasy</groupId>
        <artifactId>resteasy-jackson2-provider</artifactId>
        <version>3.9.3.Final</version>
    </dependency>
    <!--Jetty相关-->
    <dependency>
        <groupId>org.eclipse.jetty</groupId>
        <artifactId>jetty-server</artifactId>
    </dependency>
    <dependency>
        <groupId>org.eclipse.jetty</groupId>
        <artifactId>jetty-servlet</artifactId>
    </dependency>
    <dependency>
        <groupId>org.eclipse.jetty</groupId>
        <artifactId>jetty-util</artifactId>
    </dependency>

> tips:  
> 1.[springfox-swagger-extension-core](https://mvnrepository.com/artifact/com.github.luffytalory/springfox-swagger-extension-core )不建议使用1.0.3之前版本，1.0.3之前在构建artifact有依赖冲突问题，建议使用1.0.3之后版本  
> 2.如果不使用dubbo rest，不需要引入dubbo rest相关依赖和jetty依赖  
> 3.可以选择使用alibaba/apache dubbo，需要引入对应的依赖即可，该demo仅展示apache
> dubbo

## 2.配置文件
server.port=9099  
spring.resources.static-locations=classpath:/**  
#entable debug  
logging.level.root=debug  
dubbo.application.name=demo-provider  
dubbo.application.id=demo-provider  
dubbo.registry.address=127.0.0.1:2181  
dubbo.registry.protocol=zookeeper  
dubbo.protocols.dubbo.name=dubbo  
dubbo.protocols.dubbo.port=8555  
dubbo.protocols.rest.name=rest  
dubbo.protocols.rest.port=8666  


>tips:  
>1.项目使用zookeeper作为注册中心，服务端默认端口2181  
>2.dubbo 协议定义了dubbo和rest，根据自己需要进行配置

## 3.启动类注解配置
>@SpringBootApplication springboot工程注解  
>@EnableDubbo  开启dubbo注解  
>@EnableDubboConfig 启用dubbo配置注解  
>@EnableDubboSwagger 启用springfox-swagger-extension-core注解  

## 4.service配置
@Api  
public interface SayHi {  

    @ApiOperation(value = "echoByInput")
    @Consumes({MediaType.APPLICATION_JSON, MediaType.APPLICATION_ATOM_XML})
    @Produces({MediaType.APPLICATION_JSON, MediaType.APPLICATION_ATOM_XML})
    String echoByInput(String s, BindingResult bindingResult);
}

@Service(protocol = {"dubbo", "rest"}, interfaceClass = SayHi.class)  
@org.springframework.stereotype.Service()  
@Path("sayHi")  
public class SayHiImpl implements SayHi {

    @Override
    @Path("echo")
    @GET
    public String echoByInput(String s, BindingResult bindingResult) {
        return s;
    }

}
> 接口SayHi，所有注解均使用swagger自带注解，不同的是在实现类SayHiImpl需要增加注解@Service(apache
> dubbo包下)，javax.ws.rs包注解@Path用来标识rest的访问路径，@GET标识请求方式，如果不用rest,可以不使用@PATH

## 5.接口文档效果图
![效果图](https://github.com/luffytalory/springfox-swagger-extension-demo/blob/master/render.png)
## 6.support
>  If you have no any question, you can send email to
>  **talory520@sina.cn**, It's my pleasure to help you to resolve use
>  problem.

 


 
