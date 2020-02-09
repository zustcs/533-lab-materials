# Swagger的使用

## 何为Swagger

设计是API开发的基础。Swagger使API设计变得轻而易举，为开发人员，架构师和产品所有者提供了易于使用的工具。——官网

1）具有准确的API模型

API设计容易出错，在建模API时发现和纠正错误是非常困难和耗时的。Swagger Editor是第一个使用OpenAPI规范（OAS）设计API的编辑器，并且继续满足开发人员使用OAS构建API的需求。编辑器实时验证您的设计，检查OAS合规性，并随时提供视觉反馈。

2）在设计时可视化

最好的API是针对最终消费者而设计的。像Swagger编辑器和SwaggerHub这样的Swagger工具为YAML编辑器提供了一个可视化面板，供开发人员使用，并查看API对最终消费者的外观和行为。 

3）在整个团队中标准化设计风格

提供共享通用行为，模式和一致的RESTful接口的API将极大地简化构建它们的人员和想要使用它们的消费者的工作。SwaggerHub配备了内置的API标准化工具，可以使您的API符合您的组织设计指南。 

## 添加依赖

**1、pom.xml：**

在此添加如下依赖

```
<dependency>
	<groupId>com.spring4all</groupId>
	<artifactId>spring-boot-starter-swagger</artifactId>
	<version>1.5.1.RELEASE</version>
</dependency>
<dependency>
	<groupId>io.springfox</groupId>
	<artifactId>springfox-swagger-ui</artifactId>
	<version>2.8.0</version>
</dependency>
```

**2、启动类：**

在Spring Boot项目启动类上添加@EnableSwagger2注解

## 类配置

```

import org.springframework.beans.factory.annotation.Value;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

import springfox.documentation.builders.ApiInfoBuilder;
import springfox.documentation.builders.PathSelectors;
import springfox.documentation.builders.RequestHandlerSelectors;
import springfox.documentation.service.ApiInfo;
import springfox.documentation.spi.DocumentationType;
import springfox.documentation.spring.web.plugins.Docket;
import springfox.documentation.swagger2.annotations.EnableSwagger2;

/**
 * Swagger2 的配置注入
 * @author ligj
 *
 */
@Configuration
@EnableSwagger2
public class Swagger2Config {
    @Value("${swagger.enable}")
    private boolean enable;
	@Bean
    public Docket createRestApi() {
		
        return new Docket(DocumentationType.SWAGGER_2).enable(enable)
                .apiInfo(apiInfo())
                .select()
                .apis(RequestHandlerSelectors.basePackage("com.ccw.service.student.rest.api.controller"))
                .paths(PathSelectors.any())
                .build();
    }

    private ApiInfo apiInfo() {
        return new ApiInfoBuilder()
                .title("你的title")
                .description("xxx 项目RestAPI文档")
                .termsOfServiceUrl("...")
                .version("1.0")
                .build();
    }

	
}
```

## 常用API

常用注解有：

1、Api

2、ApiModel

3、ApiModelProperty

4、ApiOperation

5、ApiParam

6、ApiResponse

7、ApiResponses

8、ResponseHeader



### Api注解

Api用在类上，说明该类的作用，可以标记一个Controller类作为swagger文档资源，标记在类上，使用方式：

```
@Api(tags = "信息")
public class Controller{
	...
}
```

属性配置

| 属性名称       | **备注**                                         |
| -------------- | ------------------------------------------------ |
| value          | url的路径值                                      |
| tags           | 如果设置这个值，value的值会被覆盖                |
| description    | 对api资源的描述                                  |
| basePath       | 基本路径可以不配置                               |
| position       | 如果配置多个Api 想改变显示的顺序位置             |
| produces       | For example, "application/json, application/xml" |
| consumes       | For example, "application/json, application/xml" |
| protocols      | Possible values: http, https, ws, wss.           |
| authorizations | 高级特性认证时配置                               |
| hidden         | 配置为true 将在文档中隐藏                        |

### ApiOperation注解

用在方法上，说明方法的作用，每一个url资源的定义，使用方式：

```
@ApiOperation(value = "your describle")
@RequestMapping(value = "/xxx",method = RequestMethod.POST, produces = "application/json;charset=UTF-8")
public RestResponseModel getSongByUnit(@RequestBody @Validated UnitDTO unitDTO, @ApiIgnore ReqUser reqUser){
    your code
}
```

属性配置

| 属性名称          | 备注                                                         |
| ----------------- | ------------------------------------------------------------ |
| value             | url的路径值                                                  |
| tags              | 如果设置这个值、value的值会被覆盖                            |
| description       | 对api资源的描述                                              |
| basePath          | 基本路径可以不配置                                           |
| position          | 如果配置多个Api 想改变显示的顺序位置                         |
| produces          | For example, "application/json, application/xml"             |
| consumes          | For example, "application/json, application/xml"             |
| protocols         | Possible values: http, https, ws, wss.                       |
| authorizations    | 高级特性认证时配置                                           |
| hidden            | 配置为true 将在文档中隐藏                                    |
| response          | 返回的对象                                                   |
| responseContainer | 这些对象是有效的 "List", "Set" or "Map".，其他无效           |
| httpMethod        | "GET", "HEAD", "POST", "PUT", "DELETE", "OPTIONS" and "PATCH" |
| code              | http的状态码 默认 200                                        |
| extensions        | 扩展属性                                                     |

### ApiParam注解

ApiParam为请求属性，使用方式：

```
public ResponseEntity<User> createUser(@RequestBody @ApiParam(value = "Created user object", required = true)  User user)
```

属性配置

| 属性名称        | 备注         |
| --------------- | ------------ |
| name            | 属性名称     |
| value           | 属性值       |
| defaultValue    | 默认属性值   |
| allowableValues | 可以不配置   |
| required        | 是否属性必填 |
| access          | 不过多描述   |
| allowMultiple   | 默认为false  |
| hidden          | 隐藏该属性   |
| example         | 举例子       |

### ApiModelProperty注解

描述一个model的属性，具体用法如下：

```
@ApiModelProperty(value = "your describle")
private String id;
```