1. AspectJ注解
- @Aspect 用来标志类是用于切面
- @Pointcut 
- Advice:@Before;@After

2. 切面表达式
- designators指示器：
+ within():传入类的全名，则匹配这个类中的所有方法：
```@Pointcut("within(com.abc.service.ProductService)")
```@Pointcut("within(com.abc..*)")
+ 匹配对象：this(),target(),bean()
+ this():匹配指定对象的代理对象的方法
+ target():