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
```
my.directive('sayHello',function(){
	return {
		restrict:'E',
		replace:true,
		template:'<h1>say hello to {{name}}</h1>',
		link:function($scope,ele,attr){
			$scope.name = attr.name;
		}
	}
})
```
其中，link方法中的参数分别为：$scope 表示作用域，如果定义了一个控制器，作用域就是这个控制器的作用域，否则就是根作用域，ele 指当前元素， 
attr 表示一个对象，包含指令的所有属性和参数。
### compile方法
1. Angularjs处理指令的过程：
首先是浏览器开始渲染HTML文档，创建DOM，加载完成后会触发onload事件，我们通过<script>标签将AngularJS框架引入到页面中，它就会监onload事件，
然后从DOM中查找ng-app指令，找到后就开启AngularJS框架，我们使用directive自定义的指令都在一个容器中，Angularjs会进行识别和处理，一旦处理
完成就会执行compile方法。所以只会调用一次。
```
<div repeat = 5>
	<div> <h1>刘暾是超级无敌大坏蛋</h1></div>
</div>

<script type="text/javascript">
		var my = angular.module('myM',[]);
		my.directive('repeat',function(){
			return {
				restrict:'A',
				replace:true,
				compile:function(tele,tattrs){
					var ele = tele.html(); 
					for(var i=0;i<tattrs.repeat;i++){
						tele.append(ele);
					}
					console.log('完成');
				}
			}
		})
</script>
```
  
## 自定义指令的作用域
1. 自定义指令作用域与父级作用域的关系默认情况下，指令使用其父级作用域` <div ng-controller='myController'> <hello-world></hello-world> </div>`
自定义指令hello-world作用域如果不做处理，其实例化的作用域与控制器实例化的作用域是同一个作用域对象。但是有时候并不需要这样，如果我们需要更改自定
义指令的作用域，会修改了父级作用域的值。因此想要摆脱父级作用域有两种方法：
* 使用子作用域原型继承父作用域
```
app.directive('helloWorld',function(){
      restrict:'',
      replace:true,
      scope:true,
      template:
})
```
* 使用孤立作用域，创建一个新的作用域，和父作用域没有任何关系
```
app.directive('helloWorld',function(){
      restrict:'',
      replace:true,
      scope:{},
      template:
})
```
2. 孤立作用域与父作用域绑定
孤立作用域无法继承父作用域，为了解决这个问题，提供了三种绑定方法：
* 使用@符号建立基于属性的绑定：
需要在自定义的指令中添加一个属性，属性值可以使表达式获取控制器作用域中的值
```
<div ng-controller='myC'>
    <p>用户名：<input type="text" ng-model='texts' /></p>
    <hello-world attr-name='{{texts}}'></hello-world>
</div>

<script type="text/javascript">
   var my = angular.module('myM',[]);
   my.controller('myC',function($scope){
       $scope.texts = 'lchjiao';
   });
   my.directive('helloWorld',function(){
       return {
          restrict:'E',
          replace:true,
          scope:{
              texts:'@attrName'
          },
          template:'<span>善良的{{texts}}</span>'
        }
   })
</script>
```
* 使用=建立双向数据绑定
使用@的绑定是单向的，而且无法对对象/数组这些复杂的数据模型进行绑定，使用=可以进行双向绑定
```
<div ng-controller='myC'>
		 <p>用户名：<input type="text" ng-model='texts' /></p>
		 <hello-world attr-name='texts'></hello-world>
</div>
<script type="text/javascript">
    var my = angular.module('myM',[]);
    my.controller('myC',function($scope){
         $scope.texts = 'lchjiao';
    });
    my.directive('helloWorld',function(){
         return {
             restrict:'E',
             replace:true,
             scope:{
                    texts:'=attrName'
              },
              link:function(scope,ele,attr){
                    scope.texts = 'Jan';
              },
              template:'<span>善良的{{texts}}</span>'
         }
    })
</script> 
```
* 使用&符号调用父级作用域中的方法






  

  
