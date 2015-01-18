----------------------------
Вт. окт. 28 21:14:11 MSK 2014

https://github.com/homerjam/angular-images-loaded

----------------------------
Вт. июля  1 17:45:47 MSK 2014

.run(function ($rootScope, $location, $http) {
  $http.get('/api/config').success(function(config) {
    $rootScope.config = config;
  });
});
-----------------


$scope.$apply();

ng-class="{selected: $index==selectedIndex}"

-------------------------

http://www.angular.ru/guide/directive

http://habrahabr.ru/post/179473/

https://github.com/jmcunningham/AngularJS-Learning

-------------------

$httpBackend.whenGET("http://api.example.com/data").respond({'data': 123});

РЕСТ дёргаем
var commits = $resource('https://api.github.com/repos/:user/:project/commits').get({user: 'mkotsur', project: 'gitoscop'})

 ng-app=«admin» тега <html> и раздел <div ng-view></div>

angular.module('admin', ['admin.services','admin.filters'])
  .config(['$routeProvider', function($routeProvider) {
    $routeProvider
      .when('/list', {template: 'views/list.html', controller: ListCtrl})
      .when('/new', {template: 'views/edit.html', controller: NewCtrl})
      .when('/edit/:id', {template: 'views/edit.html', controller: EditCtrl})
      .otherwise({redirectTo: '/list'});
  },
]);

<span class="disable-item" style="color:{{['red','green'][+item.shown]}};" ng-click="disableItem()">{{['выкл','вкл'][+item.shown]}}</span>

--------------

<div ng-init="list = ['Chrome', 'Safari', 'Firefox', 'IE'] ">
  <input ng-model="list" ng-list> <br>
  <input ng-model="list" ng-list> <br>
  <pre>list={{list}}</pre> <br>
  <ol>
    <li ng-repeat="item in list">
      {{item}}
    </li>
  </ol>
</div>

--------------

{{ list | filter:predicate | json }}

------------
Формат числа: {{ 1234567890 | number }} <br>

-----------

angular.module('timeExampleModule', []).
  // Объявим новый, доступный для инъекций объект
  // и назовем его time
  factory('time', function($timeout) {
    var time = {};

    (function tick() {
      time.now = new Date().toString();
      $timeout(tick, 1000);
    })();
    return time;
  });


// Обратите внимание на то, что можно просто запросить time
// и запрос будет удовлетворен. Не нужно ничего искать.
function ClockCtrl($scope, time) {
  $scope.time = time;
}
