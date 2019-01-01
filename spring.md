## spring 笔记

#### spring依赖注入 一个接口多个实现
1. @Autowired + @Qualifier("component1")
2. @Resource(name="component1")
3. @Autowired list形式注入
        @Autowired
        List<Component> components

#### springboot命令行以不同分支启动
    java -jar x.jar --spring.profiles.active=xxx

#### springboot启动忽略数据源的配置
在springboot项目中如果不配置数据源的话会报以下的错误:
![avatar](./image/spring-sb-data.png)
解决方案:
1、pom删除所有的数据源依赖
2、@EnableAutoConfiguration(exclude={DataSourceAutoConfiguration.class})启动类排除数据源检查