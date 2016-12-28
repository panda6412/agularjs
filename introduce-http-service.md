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



