## AngularJS Resources

You may have noticed that many introductions to AngularJS have code examples that place their Javascript logic in an Angular controller. While this is fine when you are first learning how to use AngularJS, this anti-pattern of dumping your Javascript into a controller will quickly become unmaintainable in a real world application.

One tool that I've started to use more often is Angular resources. They provide a simple pattern for integrating your Angular application with an REST-ful API and have the added bonus of making it easier to Unit test your Angular application.

Consider the following example:

```javascript
angular.controller("UserController", function ($scope, $http) {
  $http.get('/user/'+$routeParams.id).then(function (user) {
    $scope.user = user;
  });


});

```
