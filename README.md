# AngularJS 自定义指令

## 定义
```
app.restrict('helloWorld',function(){
  return {
    restrict:'AEC',
    replace:true,
    template:'<a href="http://www.baidu.com">点击去百度</a>'
  };
});
```
restrict属性：约束我们自定义指令以什么形式出现，Angularjs指令有四种形式，A（属性） E(元素) C（样式） M（注释）。

replace属性：该属性是定义是否使用template属性定义的HTML模板内容替换指令所在的HTML内容。

template属性：用于指定Angularjs指令替换的HTML模板。

使用时:
```
<hello-world></hello-world>
<hello:world></hello:world>
<span hello-world></span>
<span hello:world></span>
```

directive（）方法有两个参数，第一个参数为指令名称，第二个参数为指令方法，该方法返回一个对象，即指令定义对象。
## 指令定义对象
### link方法
* 需要访问指令的属性时，可以用到link方法，需要获取或者修改自定义指令的属性时
* 指令需要监视AngularJS作用域模型数据变化时
* 当指令需要响应HTML模板中元素点击、鼠标移动事件时
#### 例1：
       
  

  
