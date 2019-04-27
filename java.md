## java 笔记

### java语言
#### 集合
for、增强for循环遍历中不能删除元素
解决方案： 使用迭代器删除

**collection 转 ArrayList等子类：**
错误：(List)collection  异常
正确：使用构造器：new ArrayList(collection)

### spring
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

#### feign调用RequestMapping不能被继承

```
父接口：
public interface BaseClient {
    @RequestMapping(value = "/save", method = RequestMethod.POST)
    String query(@RequestBody User user); 
}
子接口
@FeignClient(name = "${service.name}", url="${service.url}")
@RequestMapping("/user")
public interface UserClient extends BaseClient{
}
```
这样的情况使用/user/save，是失败的
解决方案：使用 FeignClient的path
```
@FeignClient(name = "${service.name}", url="${service.url}", path="/user")
public interface UserClient extends BaseClient{
}
```

#### spring aop
spring对类内部调用的方法调用是aop不起作用
```
class A {
    public void t1() {
        // 如果对class A进行代理，只有方法t1能被代理，t2不行
        t2();
    }
    public void t2() {}
}
```

### mybatis
####  resultMap与 resultType
resultType:
可以是java自带的类型，如：string、integer、map等，list也需要配置成map。但是在自定义的类型时，需要数据库查询的结果集合一样。否则不能自动注入。
resultMap:
可以由自定的数据映射规则，例：
```
<resultMap id="user" type="com.xxx.user">
// id映射主键信息
<id column="id" property="id" />
<result column="u_name" property="name" />
<result column="u_age" property="age" />
</resultMap>
```
####插入 返回自增id
sql的xml文件上：useGeneratedKeys="true" keyProperty="id" (java bean 对应的字段)

####resultType使用Map接受泛型接受无效
xml中的sql：
```
<select id="findDome" resultType="java.util.HashMap">
SELECT count(*) as total sum(fail_nums) as fails,from dome
</select>
// Dao中接口：
Map<String,Integer> findDome()
```
在接受到map的结果：
![avatar](./image/mybatis-result_type1.png)

在结果mybatis没有做类型转换的Map中除了Integer还有BigDecimal类型，但是有泛型的情况下，如果其他类型应该报错才对，但是并没有。也就是说这里的泛型并没有起作用。
泛型之所以没起作用是因为泛型擦除了，在运行时没有泛型的存在，相应接受的Map中就可以接受任何类型

### dubbo
dubbo注册当中，url参数指的是不从zk上拉取服务，从指定的url地址直连
```
// check 启动时是否对bean进行检查，true：进行检查如果bean未初始化，启动失败
<dubbo:reference id="xx" url="" interface="com.aa.bb" check="false" />
```

### java Comparator
在实现comparator时发生异常：
实现过程：
```
Collections.sort(liveServers, (o1, o2) -> {
    if (o1 == null) {
        return -1;
    }
    if (o2 == null) {
        return 1;
    }
    return o2.compareTo(o1);
});
```
异常信息：
```
java.lang.IllegalArgumentException: Comparison method violates its general contract!
```
原因：在Jdk1.7之后，实现comparator时需要遵守：自反性、传递性、对称性。即：

- 自反性：x，y 的比较结果和 y，x 的比较结果相反。
- 传递性：x>y,y>z,则 x>z。
- 对称性：x=y,则 x,z 比较结果和 y，z 比较结果相同。 

在上面的实现当中，违反了自反性。当o1 == null的时候返回-1，说明o1小于o2。但是当02 == null时，返回1，说明o2小于o1。这显然是不符对称性的。

修改结果：
```
Collections.sort(liveServers, (o1, o2) -> {
    if (o1 == null && o2 == null) {
        return 0;
    }
    if (o1 == null) {
        return -1;
    }
    if (o2 == null) {
        return 1;
    }
    return o2.compareTo(o1);
});
```