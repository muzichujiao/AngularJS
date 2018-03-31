# AngularJS过滤器
## 过滤器的使用
1. 在表达式中 `{{expression | filter1 | filter2}}`
2. 在指令中使用 `<span ng-repeat = 'element in arr | filter'> </span>`
3. 在Controller或者AngularJS服务中使用
```
app.controller('myCon',function($scope,currencyFilter){  // 在这个控制器中我们使用currencyFilter过滤器将数组转化为金额
  $scope.amout = currencyFilter(100123); 
})
```
如果使用的过滤器过多，会造成参数过多，但是Angularjs提供了一种简单的方法$filter服务，它可以调用所有的过滤器。
```
app.controller('myCon',function($scope,$filter){
  $scope.amout = $filter('currency')(1000022);
  $scope.data = $filter('date')(new Date(),'yyyy-MM-dd hh:mm:ss');
})
```
## AngularJS内置过滤器
### filter过滤器
1. 可以用来处理数组，过滤出含有某个子串的元素，将过滤的元素作为数组返回。也可以接受一个参数，用来定义子元素的匹配格式。
```
<div ng-controller='myCont'>
		<input type="text" ng-model='infor' />
		<ul>
			<li ng-repeat='person in people | filter:infor'>姓名：{{person.name}},年龄{{person.age}},性别{{person.sex}}</li>
		</ul>
</div>
<script type="text/javascript">
		var app = angular.module('myApp',[]);
		app.controller('myCont',function($scope){
			$scope.people = [
			{name:'lchj',age:18,sex:'女'},
			{name:'lt',age:19,sex:'男'},
			{name:'mm',age:19,sex:'男'},
			{name:'wxy',age:19,sex:'女'},
			{name:'wld',age:20,sex:'女'},
			{name:'sjw',age:20,sex:'男'},
			];
		}) ;
</script>
```
### currency过滤器
可以将数字转化为货币表示，默认是美元符号，也可以设置，同时还可以设置精确度
`{{amout|currency:'￥'}}` 或者 `{{amout|currency:'￥':1}}`
### number过滤器
用于将数字格式文本化，同时可以控制数字精度`{{index|number:4}}`
### lowercase和uppercase过滤器
相对的过滤器，分别是将字符串中大写字母转化成小写字母和将小写字母转化成大写字母
### date过滤器
将日期格式化，该过滤器接收一个参数，为日期字符串格式。
`{{now|date:'yyyy年MM月dd日'}}` 或者 `{{now|date:'yyyy-MM-dd hh:mm:ss'}}`
### json过滤器
该过滤器不接受任何参数，将js对象转化为json字符串
### limitTo过滤器
用来截取数组或者是字符串，接收一个参数来指定截取的长度，如果参数是负数，则从尾部开始截取。
```
	<div ng-controller='myCont'>
		<p>截取数组前两个元素：</p>
		<ul>
			<li ng-repeat='person in people|limitTo:2'>{{person}}</li>
		</ul>
		<p>截取字符串前7个元素</p>
		<p>{{message|limitTo:7}}</p>
	</div>
	<script type="text/javascript"> 
		var app = angular.module('myApp',[]);
		app.controller('myCont',function($scope){
			$scope.message = "Welcom to my home!";
			$scope.people = [
			{name:'lchj',age:18,sex:'女'},
			{name:'lt',age:19,sex:'男'},
			{name:'mm',age:19,sex:'男'},
			{name:'wxy',age:19,sex:'女'},
			{name:'wld',age:20,sex:'女'},
			{name:'sjw',age:20,sex:'男'},
			];
		})
```
### orderBy过滤器
将数组中元素进行排序，接收一个参数作为排序的规则，可以是字符串，表示以该属性名进行排序，可以是一个方法，定义排序属性，也可以是一个数组，表示依次按数组中的属性值进行排序。
### 自定义过滤器
使用filter方法自定义过滤器，结构
```
app.filter('myFilter',function(){ // fliter有两个参数，一个是过滤器的名称，一个是过滤方法，该方法需要返回一个处理方法
  return function(input,num,...){ //该处理方法第一个参数为待过滤的数据，后面的参数表示过滤的参数
    …… ……
    return result； //返回过滤后的数据
  }
})
```
例如：
```
<div ng-controller='myCont'>
		<input type="text" ng-model='price' />
		<table>
			<thead>
				<tr>
					<th>姓名</th>
					<th>年龄</th>
					<th>性别</th>
				</tr>
			</thead>
			<tbody>
				<tr ng-repeat='person in people|search:price'>
					<td>{{person.name}}</td>
					<td>{{person.age}}</td>
					<td>{{person.sex}}</td>
				</tr>
			</tbody>
		</table>

	</div>
	<script type="text/javascript">
		var app = angular.module('myApp',[]);
		app.controller('myCont',function($scope){
			$scope.price = 23;
			$scope.people=[
			{name:'lchj',age:18,sex:'女'},
			{name:'lt',age:19,sex:'男'},
			{name:'mm',age:19,sex:'男'},
			{name:'wxy',age:19,sex:'女'},
			{name:'wld',age:20,sex:'女'},
			{name:'sjw',age:20,sex:'男'},
			];
		});
		app.filter('search',function(){
			return function(input,price){
				var result = [];
				for(var i=0;i<input.length;i++){
					if(input[i].age<price){
						result.push(input[i]);
					}
				}

				return result;
			}
		})
	</script>
```
