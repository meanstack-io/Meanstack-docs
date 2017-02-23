# Routes - AngularJS
*AngularUI Router, Module Route*

---

- [**Introduction**](#introduction)
- [**Creating Routes**](#creatingroutes)
- [**Module Route**](#moduleroute)
  - [**namespace**](#namespace)
  - [**templateNamespace**](#templateNamespace)
  - [**routeProvider**](#routeprovider)

---

## Introduction

For the routes in AngularJS we use the [AngularUI Router](http://ui-router.github.io/), and to facilitate working with the api we created a module called "route".

## Creating Routes

Creates a file in "resources/assets/javascripts/angular/routes/" with the desired name, example, "greeting.js"

```javascript
angular.module('App').config(['$stateProvider', function ($stateProvider) {

    $stateProvider.state('greeting', {
        url: '/',
        templateNamespace: 'greeting',
        data: {
            title: 'Greeting :)'
        },
        controller: 'greetingController',
        resolve: {
            deps: ['$ocLazyLoad', 'path', function ($ocLazyLoad, path) {
                return $ocLazyLoad.load({
                        insertBefore: '#load_js_before',
                        files: [
                            path.controller('greetingController')
                        ]
                    }
                );
            }]
        }
    });
}]);
```
  
* "data.title" => O "metaTags.js" Add a title to your page based on this setting.
 
* "resolve" => Resolve dependencies before loading the view.

* "$ocLazyLoad.load" => Load the dependency and inserts in the HTML.

* "path.controller" => Returns the controller path configured in "resources/assets/javascripts/angular/config.js" => "path.controllers"


## Module Route

The route module is designed to facilitate interaction with the api namespace and configure the exception pages.

### namespace

Adds the api namespace in the url, configured in "resources/assets/javascripts/angular/config.js" => "route.namespace"

```javascript
angular.module("App").factory('searchService',
    ['$http', 'route',
        function ($http, route) {
            return {
                loadPage: function (page, params) {
                    return $http.get(route.namespace('search/load/' + page), {params: params});
                }
            };
        }]
);
```

### templateNamespace

Adds the api namespace in the url template.

```javascript
angular.module('App').config(['$stateProvider', function ($stateProvider) {

    $stateProvider.state('greeting', {
        url: '/',
        templateNamespace: 'greeting',
        //...
    });
}]);
```

In this case it would be "/api/greeting"

### routeProvider

```javascript
angular.module('App')
    .config(['routeProvider', '$stateProvider', '$urlRouterProvider', 'config',
        function (routeProvider, $stateProvider, $urlRouterProvider, config) {

            /**
             * Config route module MEANStack
             */
            routeProvider.config(config.route);

            /**
             * Error routes.
             */
            $stateProvider
                .state('404', {
                    templateNamespace: 'error/404',
                    data: {title: 'Page not found'}
                })

                .state('403', {
                    templateNamespace: 'error/403',
                    data: {title: 'Not authorized'}
                })

                .state('500', {
                    templateNamespace: 'error/500',
                    data: {title: 'Internal server error'}
                });

            /**
             * Error 404.
             * If requested route not registered.
             */
            $urlRouterProvider.otherwise(function ($injector, $location) {
                $injector.get('$state').go('404');
                return $location.path();
            });
        }]);
```

* Config route module.
* Config page 404, 403 and 500
* Config requested route not registered go to 404

By default routes 404, 403 and 500 are already registered in case the server retries these errors will be directed to those pages. If you return an error that is not registered, you will be directed to page 500.
If you need to register more pages add the stateProvider and pass her code on "route.bootstrap" of "resources/assets/javascripts/angular/run.js".

* route.bootstrap -> First parameter - error status register, Secondary parameter - error exception.
  * Default is -> [404, 403, 500], 500
  
```javascript
angular.module('App')
    .run(['route', function (route) {
        
        route.bootstrap([404, 403, 500, 408], 500); // Added error 408 Request Timeout.

    }]);
```
