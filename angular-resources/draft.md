## AngularJS Resources

You may have noticed that many introductions to AngularJS use code examples that place their Javascript logic in an Angular controller. While this is fine when you are first learning how to use AngularJS, this anti-pattern of dumping your Javascript into a controller will quickly become unmaintainable in a real world application.

One way to simplify your controllers is to use the Angular resources module. It provides a simple pattern for integrating your Angular application with an REST-ful API and has the added bonus of making it easier to Unit test your Angular application.

Consider this generic controller:

```javascript
angular.controller("UserController", function ($scope, $http) {
  // load current user
  $http.get('/user/'+$routeParams.id).then(function (user) {
    $scope.user = user;
  });

  //update user. updateUser would get called an ng-submit or ng-click action in a template
  $scope.updateUser = function (formData) {
    $http.put('/user/'+$routeParams.id, formData).then(function (user) {
      $scope.user = user;
    });
  };

  // logic that belongs to User model
  $scope.fullName = $scope.user.first_name + $scope.user.last_name;

});

```

There are a few things to note in this controller. First, we need to explicitly identify the `User` route for each api call (`'/user/'+$routeParams.id`). This isn't very DRY. Secondly, we are placing `User` model logic in our controller. Our `User` object should know what its `fullName` is. We shouldn't need to rely on the controller to call this function. This means that if we were to load `User` objects in a different controller, we would need to define `$scope.fullName` __again__. Furthermore, this coupling between the `User` model and the `UserController` makes it more difficult to test the `UserController`, because we would need to create a mock `User` object. Ideally, all `User` model logic would be contained in a Unit test.

Enter Angular Resources.

First, make sure you have the `ngResource` module loaded into your Angular application. Something along the lines of this:
```javascript
  angular.module('myAngularApp', ['ngRoute', 'ngResource']);
```

Now create a factory that returns a $resource.
```javascript
  angular.module('factories', function ($resource) {
    var User = $resource('/user/:id', {id: '@id'})

    User.prototype.fullName = function () {
      return this.first_name + this.last_name;
    };
  })
```
