# $Http service 簡介

$http 是angularjs 一個很重要的核心service 它可以幫助我們去請求後端資料以利我們在前端能夠動態展示，比較特別的是 Angularjs XHR API 是採用 promise 的介面，而promise 有一個最大的特點就是非同步處立又稱異步處理，採用這種介面可以讓使用者已預期的方式來使用它們，接下來的小節將會更深入介紹promise 。

# 何謂非同步處理 ？

所謂非同步處理亦即當有兩個任務時並不會等到前一個任務執行完才執行後面的，兩個任任務的處理時間是分開的，這樣有一個好處是不會造成任務延遲堵塞，一下有一個很經典的例子可以幫助我們了解

有一天小王遇到一個很難的問題，問題是“1 + 1 = ?”，就打電話問小李，小李一下子也不知道，就跟小王說，等我辦完手上的事情，就去想想答案，小王也不會傻傻的拿著電話去等小李的答案吧，於是小王就對小李說，我還要去逛街，你知道了答案就打我電話告訴我，於是掛了電話，自己辦自己的事情，過了一個小時，小李打了小王的電話，告訴他答案是2

# $http service Example

```
<!DOCTYPE html>
<html ng-app="noteapp">
<head>
    <title></title>
    <script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.3.11/angular.js"></script>
    <script type="text/javascript">
        angular.module('noteapp',[ ]).controller('MainCtrl',['$http', function($http){
            var self = this;
            self.items;
            self.status ;
            self.config;
            self.header ;
            $http.post('http://122.117.134.145/CodeIgniter_Practice/index.php/TravelAPI/List')
                .then(function(response){
                    self.items = response.data ;
                    self.status = response.status ;
                    self.config=response.config;
                    self.header = response.header ;
                    console.log('fetch data success!');
            },function(errResponse){
                console.log('fetch data error');
            });
        }]);
    </script>
    <style type="text/css">
        .item{
            padding:10px;
        }
        .title{
            font-size: 25px;
            color:red;

        }
    </style>
</head>
<body ng-controller='MainCtrl as ctrl'>

    <h1>Response Status is {{ctrl.status}}</h1>
    <h1>Request Config is {{ctrl.config}}</h1>
    <hr>
    <table>
        <tr class='title'>
            <td>_id</td>
            <td>stitle</td>
            <td>xbody</td>
            <td>MEMO_TIME</td>
        </tr>
        <tr ng-repeat='item in ctrl.items' class='item'>
            <td>{{item._id}}</td>
            <td>{{item.stitle}}</td>
            <td>{{item.xbody}}</td>
            <td>{{item.MEMO_TIME}}</td>
        </tr>
    </table>
</body>
</html>
```

> 這段程式碼值得需要注意的地方$http.get的方法第一個argument為url，然後除了get之外，$http還提供post、head、put、delete、jsonp，再來就是這些function 皆會回傳promise物件，因此是可以鏈結使用的，然而promise 所使用的方法就類似回呼，若有一百個請求，用戶端並不會因為等待回應而拖延運作。
>
> .then\(\)函式有兩的引數，第一個是成功處理器\(success handler\)，第二個則是錯誤處理器\(error handler\)，這兩個處理器皆會取的傳遞的回應物件，而這些物件有一些屬性可以讓我們了解回應的狀況 
>
> * headers   呼叫的標頭
>
> * status   回應的狀態碼
>
> * config    含有呼叫所產生的組態
>
> * data   來自伺服器回應的主體

```
<!DOCTYPE html>
<html ng-app='noteapp'>
<head>
    <title></title>
    <script src='https://ajax.googleapis.com/ajax/libs/angularjs/1.3.11/angular.js'></script>
    <script type="text/javascript">
        angular.module('noteapp',[]).controller('MainCtrl',['$http',function($http){
            var self = this ;
            self.items = [];
            self.newItem = {};
            var fetchItem = function(){
                return $http.get('http://122.117.134.145/CodeIgniter_Practice/index.php/TravelAPI/ListAccount')
                .then(function(response){
                        self.items = response.data;
                        console.log('fetch data success !');
                    },function(errResponse){
                        console.log('fetch data error !');
                    });
            };
            fetchItem();
            self.addItem = function(){
                $http.post('http://122.117.134.145/CodeIgniter_Practice/index.php/TravelAPI/addItem',self.newItem)
                    .then(fetchItem)
                    .then(function(response){
                        self.newItem={};
                    });
                    console.log('click already');
            };
        }])
        .factory('MyLoggingInterceptor', ['$q', function($q) {
            return {
              request: function(config) {
                console.log('Request made with ', config);
                return config;
                // If an error, or not allowed, or my custom condition
                // return $q.reject('Not allowed');
              },
              requestError: function(rejection) {
                console.log('Request error due to ', rejection);
                // Continue to ensure that the next promise chain
                // sees an error
                return $q.reject(rejection);
                // Or handled successfully?
                // return someValue;
              },
              response: function(response) {
                console.log('Response from server', response);
                // Return a promise
                return response || $q.when(response);
              },
              responseError: function(rejection) {
                console.log('Error in response ', rejection);
                // Continue to ensure that the next promise chain
                // sees an error
                // Can check auth status code here if need to
                // if (rejection.status === 403) {
                //   Show a login dialog
                //   return a value to tell controllers it has
                // been handled
                // }
                // Or return a rejection to continue the
                // promise failure chain
                return $q.reject(rejection);
              }
            };
          }])
      .config(['$httpProvider', function($httpProvider) {
        $httpProvider.interceptors.push('MyLoggingInterceptor');
      }]);
    </script>
</head>

<body ng-controller='MainCtrl as ctrl'>
    <div>
        <table>
            <tr>
                <td>id</td>
                <td>item</td>
                <td>amount</td>
            </tr>
            <tr ng-repeat='item in ctrl.items'>
                <td>{{item.id}}</td>
                <td>{{item.item}}</td>
                <td>{{item.amount}}</td>
            </tr>

        </table>
    </div>

    <div>
        <form name='addForm' ng-submit='ctrl.addItem()'>
            <input type="text" name="item" placeholder="item" ng-model='ctrl.newItem.item' required>
            <input type="text" name="amount" placeholder="amount" ng-model='ctrl.newItem.amount' required>
            <input type="submit" value="add" ng-disabled='addForm.$invalid'>
        </form>
    </div>
</body>
</html>
```



