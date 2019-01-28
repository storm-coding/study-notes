##单元测试 笔记


#### mockito
- @mock  创建一个假的对象类型于spring的autowrite，不同的是mock出来的对象都是假的，里面没有任何方法和属性。
- when(class.method(args)).thenReturn(result); when用来自定义某个方法的逻辑。
- verify(class,times(1)).method(args); verify用来验证结果，这里验证的是方法调用的次数。
  
  * times 验证方法调用的次数
  * atLeast 验证方法至少调用的次数
- MockitoAnnotations.initMocks(this);
- @Captor 验证传入的参数
     
  例:private ArgumentCaptor<List<beanName>> argument;
  verify(class, times(1)).method(argument.capture());
   assertTrue('xx' == argument.getValue());
   
      - getValue,获取最后一次传入的参数
      - getAllValue,获取传入的所有参数，list集合  

#### after中结束线程

单元测试一个线程:
```
public class TestThread extends Thread {
   public void setExit() {
	this.exit = true;
   }

   public void run() {
      while (!exit) {
      }
   }
}
```
在after中调用setExit,退出线程
```
@After
public void tearDown() throws Exception {
   testThread.setExit();
}
```
这样并不会在执行玩after之后，线程立马就结束。因为线程要下一次运行在while判断中才会结束线程。如果在whie中有等待，这样就会影响到其他单元测试的结果。
解决方案：使用join等待after执行完成。
```
@After
public void tearDown() throws Exception {
   testThread.setExit();
   testThread.join();
}
```

junit整合测试类：一次运行多个测试类
@RunWith(Suite.class)  
@Suite.SuiteClasses({   })