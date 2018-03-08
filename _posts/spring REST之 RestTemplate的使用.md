title: spring REST之 RestTemplate的使用
date: 3/8/2018 11:28:23 AM 
tags: 框架
---

## 一. 基础知识 ##

REST就是将资源的状态以最适合客户端或服务端的形式从服务器端转移到客户端（或者反过来）

在REST中是通过URL进行识别一定位。是通过HTTP方法来定义。也就是GET,POST，PUT，DELETE，PATCH等。这些HTTP方法通常会匹配如下的CRUD操作。

 - Create:POST
 - Read: GET
 - Update: PUT或PATCH
 - Delete： Delete

## 二. 基本使用 ##

在RestTemplate提供了对应于每个的六个主要的HTTP方法

表1. RestTemplate方法的概述

RestTemplate定义了36个与REST资源交互的方法，其中的大多数都对应于HTTP的方法。 
其实，这里面只有11个独立的方法，其中有十个有三种重载形式，而第十一个则重载了六次，这样一共形成了36个方法

 - delete() 在特定的URL上对资源执行HTTP DELETE操作
 - exchange() 在URL上执行特定的HTTP方法，返回包含对象的ResponseEntity，这个对象是从响应体中映射得到的
 - execute() 在URL上执行特定的HTTP方法，返回一个从响应体映射得到的对象
 - **getForEntity()** 发送一个HTTP GET请求，返回的ResponseEntity包含了响应体所映射成的对象
 - **getForObject()** 发送一个HTTP GET请求，返回的请求体将映射为一个对象
 - **postForEntity()**  POST 数据到一个URL，返回包含一个对象的ResponseEntity，这个对象是从响应体中映射得到的
 - **postForObject()** POST 数据到一个URL，返回根据响应体匹配形成的对象
 - headForHeaders() 发送HTTP HEAD请求，返回包含特定资源URL的HTTP头
 - optionsForAllow() 发送HTTP OPTIONS请求，返回对特定URL的Allow头信息
 - postForLocation() POST 数据到一个URL，返回新创建资源的URL
 - put() PUT 资源到特定的URL

## Rest服务,模拟提供Rest数据 ##

	@RestController
	public class DataController {
	    @RequestMapping(value = "getAll")
	    public List<UserEntity> getUser() {
	        List<UserEntity> list = new ArrayList<>();
	        list.add(new UserEntity("zhangsan","123456","man"));
	        list.add(new UserEntity("lisi","321654","woman"));
	        list.add(new UserEntity("wangfer","741258","man"));
	        return list;
	    }
	
	    @RequestMapping("get/{id}")
	    public UserEntity getById(@PathVariable(name = "id") String id) {
	        UserEntity userEntity = new UserEntity("zhangsan", "34524", "man");
	        return userEntity;
	    }
	
	    @RequestMapping(value = "save")
	    public String save(UserEntity userEntity) {
	        return userEntity.toString()+"保存成功";
	    }
	
	    @RequestMapping(value = "saveByType/{type}")
	    public String saveByType(UserEntity userEntity,@PathVariable("type")String type) {
	        return "保存成功,type="+type;
	    }
	}

## 1. GET请求 ##

### (1)getForEntity使用方法 ###

无参数的getForEntity

    @RequestMapping("getForEntity")
    public List<UserEntity> getAll2() {
        ResponseEntity<List> responseEntity = restTemplate.getForEntity("http://localhost:8080/getAll", List.class);
        HttpHeaders headers = responseEntity.getHeaders();
        HttpStatus statusCode = responseEntity.getStatusCode();
        int code = statusCode.value();
        List<UserEntity> list = responseEntity.getBody();
        System.out.println(list.toString());
        return list;

    }

有参数的 getForEntity 请求,参数列表 如下才是参数的正确使用方式,曾在这踩了一个坑,浪费了好长时间,

    @RequestMapping("getForEntity/{id}")
    public UserEntity getById2(@PathVariable(name = "id") String id) {
        String url = "http://localhost:8080/get/{id}?username={username}&password={password}&sex={sex}";
        Map<String,String> map = new HashMap<>();
        map.put("id","11");
        map.put("username","lisi");
        map.put("password","12345");
        map.put("sex","man");
        ResponseEntity<UserEntity> responseEntity = restTemplate.getForEntity(url, UserEntity.class, map );
        UserEntity userEntity = responseEntity.getBody();
        return userEntity;
    }

> 但是,通常情况下我们并不想要Http请求的全部信息,只需要相应体即可.对于这种情况,RestTemplate提供了 getForObject() 方法用来只获取 响应体信息.  
> getForObject 和 getForEntity 用法几乎相同,指示返回值返回的是 响应体,省去了我们 再去 getBody() 

### (2)getForObject使用方法  ###

无参数的getForEntity
 	
	@RequestMapping("getAll2")
    public List<UserEntity> getAll() {
        List<UserEntity> list = restTemplate.getForObject("http://localhost:8080/getAll", List.class);
        System.out.println(list.toString());
        return list;
    }

有参数的 get 请求,使用map封装请求参数

    @RequestMapping("get3/{id}")
    public UserEntity getById3(@PathVariable(name = "id") String id) {
        HashMap<String, String> map = new HashMap<>();
        map.put("id",id);
        UserEntity userEntity = restTemplate.getForObject("http://localhost:8080/get/{id}", UserEntity.class, map);
        return userEntity;
    }

## 2. Post请求 ##

post 请求,提交 UserEntity 对像

    @RequestMapping("saveUser")
    public String save(UserEntity userEntity) {
        ResponseEntity<String> responseEntity = restTemplate.postForEntity("http://localhost:8080/save", userEntity, String.class);
        String body = responseEntity.getBody();
        return body;
    }

有参数的 postForEntity 请求,使用map封装

    @RequestMapping("saveUserByType2/{type}")
    public String save3(UserEntity userEntity,@PathVariable("type")String type) {
        HashMap<String, String> map = new HashMap<>();
        map.put("type", type);
        ResponseEntity<String> responseEntity = restTemplate.postForEntity("http://localhost:8080/saveByType/{type}", userEntity, String.class,map);
        String body = responseEntity.getBody();
        return body;
    }

以上get 和post 两个 请求基本上可以满足我们的大部分需求,如果不满足可以使用 **exchange()**或者 **execute()** 来实现,通过看源代码,可以发现,getForEntity() 和postForEntity()是对这两个方法的封装.

## 三. 手动指定转换器(HttpMessageConverter) ##

我们知道 ,reseful接口传递的数据内容和响应都是json格式的字符串.而postForObject方法请求和返回的参数都是java类,是RestTemplate通过HttpMessageConverter帮我们做了转换的操作.

通过查看restTemplate实例对象,

![](http://7xpw00.com1.z0.glb.clouddn.com/restTemplateObject.png)

默认情况下RestTemplate自动帮我们注册了一组HttpMessageConverter用来处理一些不同的contextType的请求.如:

 - StringHttpMessageConverter来处理text/plain; 
 - MappingJackson2HttpMessageConverter来处理application/json;
 - Jaxb2RootElementHttpMessageConverter来处理application/xml,text/xml。
 - AllEncompassingFormHttpMessageConverter来处理application/x-www-form-urlencoded。

这些可满足大部分的需求,如果这些都不能满足.你可以实现org.springframework.http.converter.HttpMessageConverter接口自己写一个。详情参考官方api。