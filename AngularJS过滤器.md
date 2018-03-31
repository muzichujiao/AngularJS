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
