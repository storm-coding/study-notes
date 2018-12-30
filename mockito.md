##mockito


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