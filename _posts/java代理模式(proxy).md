title: java代理模式（proxy）
date: 1/8/2016 10:09:53 AM 
tags: java
---

 **代理模式的应用场景**
> 如果已有的方法在使用的时候需要对原有的方法进行改进，此时有两种办法：

> 1、修改原有的方法来适应。这样违反了“对扩展开放，对修改关闭”的原则。
> 
> 2、就是采用一个代理类调用原有的方法，且对产生的结果进行控制。这种方法就是代理模式。
> 
> 使用代理模式，可以将功能划分的更加清晰，有助于后期维护！

 **关系图如下**

 ![关系](http://7xpw00.com1.z0.glb.clouddn.com/imageQQ%E6%88%AA%E5%9B%BE20160108101216.png)
 

 **实现代码**
	Proxy.java 代理类
	public class Proxy implements Sourceable{

	 private Source source;
	 
	 public Proxy(){  
	        super();  
	        this.source = new Source();  
	 }  
	
	@Override
	public void method() {
		// TODO Auto-generated method stub
		  before();  
	      System.out.println("111");
		  source.method();  
	      
	      atfer();  
	}
	
	private void atfer() {  
        System.out.println("after proxy!");  
    }  
    private void before() {  
        System.out.println("before proxy!");  
    }  
	}

	Sourceable.java 父接口
	public interface Sourceable {

		public void method();
	}

	Source.java 实现类
	public class Source implements Sourceable {
	@Override
	public void method() {
		// TODO Auto-generated method stub
		 System.out.println("the original method!");
	}
	}

	ProxyTest.java 测试类
	public class ProxyTest {

	public static void main(String[] args) {
		// TODO Auto-generated method stub

		 Sourceable source = new Proxy();  
	     source.method();  
	}
	}

	输入如下：
	before proxy!
	the original method!
	after proxy!

 总结：代理没有直接实现你要调用的方法，通过代理类来调用，在通过代理类的时候做一些其他的操作