# AngularJS 1.5+ Enterprise Styleguide

Lessons learned from dozens of Angular enterprise projects.

* [EcmaScript 5](#ecmascript-5)
* [HTML](#html)
    * [HTML5 Conformity](#html5-conformity)
    * [General](#general)
* [AngularJS](#angularjs)
    * [Folder Structure](#folder-structure)
    * [Names](#names)
        * [Modules](#modules)
        * [Layers](#layers)
        * [States](#states)
        * [Components, Directives and Filters](#components-directives-and-filters)
    * [Databinding](#databinding)
    * [Module](#module)
    * [Dependency Injection](#dependency-injection)
    * [Components](components)
        * [IIFE](#iife)
        * [Constants](#constants)
        * [Component](#component)
        * [Directives](#directives)
            * [Element Directives](#element-directives)
            * [Attribute Directives](#attribute-directives)
        * [Filter](#filter)
        * [Controller](#controller)
        * [Services](#services)
            * [Service pattern](#service-pattern)
            * [Resource Model Pattern](#resource-model-pattern)
    * [Promises](#promises)
    * [Scope, Watches, Emits, Broadcasts](#scope-watches-emits-broadcasts)
    * [Wrapper](#wrapper)
    * [Third-party libraries](third-party-libraries)
    * [Software Engineering Principles](software-engineering-principles)

## EcmaScript 5

[Airbnb](https://github.com/airbnb/javascript/tree/es5-deprecated/es5).

## HTML

### HTML5 Conformity

If you want to reach conformity with HTML5 you will need to do the following:

1. Non-HTML5 attributes must have a `data-` prefix.

2. Non-HTML5 tags must contain a `-`. Normally, this is achieved via a project prefix `ui-` for Angular Bootstrap components.

If you don't care about HTML5 conformity, you won't need to do this.

### General

1. The order of attributes should be consistent and preferably:

    ```
    id > class > name > custom > third-party > Angular > validation
    ```

    For example:

    ```html
    <input id="foo"
        class="bar"
        data-ak-foo
        data-ui-foo
        data-ng-bind
        required>
    ```

2. Elements with attributes on multiple lines should follow this pattern:

    ```html
    <input data-ng-bind=""
        data-ng-required="">
    ```

3. Empty elements should close on the same line:

    ```html
    <!-- Wrong -->
    <span data-ng-bind=""
        data-ng-required="">
    </span>

    <!-- Correct -->
    <span data-ng-bind=""
        data-ng-required=""></span>
    ```

4. Multiple elements on the same indentation level should be separated visually by newlines:

    ```html
    <div></div>

    <div></div>

    <div></div>
    ```

## AngularJS

### Folder structure

Keep a flat feature-based structure:

```
account/
    account.js
    account.ui.js
    account.html
```

Don't use a functional structure:

```
services/
    account.js
```

Avoid directories with general names if possible:

```
common/
utility/
```

### Names

#### Modules

You should decide on a project prefix, e.g. `ak` and apply the following hierarchical conventions:

```js
angular
    .module('ak');

angular
    .module('ak.foo');

angular
    .module('ak.foo.bar');
```

#### Layers

Split your files into representation and logic containers.

1. Submodules with view components are named `prefix.name.ui` located in `name.ui.js`, e.g. `ak.customer.ui` in `customer.ui.js`
2. Submodules with model or logic components: `prefix.name` located in `name.js`, e.g. `ak.customer` in `customer.js`

Representation components are typically `directive`, `component`, `filter` and States with the notable exception of `service` for modals.

Logic components are typically `service` and do either plain logic or API communication or a mixture of both.

#### States

States are very tied to modules, e.g.

```js
    angular
        .module('ak.customers.ui', [
            'ui.router'
        ])
        .config(route);

    function route($stateProvider) {
        $stateProvider
            .state('ak.customers', {
                url: '/customers'
            });
    }
```

#### Components, Directives and Filters

These components always have the project prefix, e.g. `akCustomers`:

```js
angular
    .module('ak.footer.ui', [])
    .component('akFooter', {
        templateUrl: 'app/footer/footer.html'
    });
```

so that it can be used in markup with `<ak-footer></ak-footer>`.

## Databinding

Prefer `ngBind` instead of `{{}}`:

```html
<!-- Wrong -->
<span>{{::vm.foo}}</span

<!-- Correct -->
<span ng-bind="::vm.foo"></span>
```

## Module

Don't bind the module to a variable since this leads to indirection. Instead use chaining:

```js
angular
    .module('foo', [])
    .constant('FOOBARBAZ' FOOBARBAZ)
    .filter('fooBaz', fooBaz)
    .controller('BarController', BarController)
    .directive('fooBar', fooBar);
```

Ideally, you'd sort it in the order of usage.

## Dependency Injection

Don't use manual dependency injection since this can be done by `ngAnnotate`.

If there's an edge case where `ngAnnotate` won't inject the dependencies you can use the `ngInject` instruction for the plugin:

```js
var foo = {
    bar: function (Customer) {
        'ngInject';
    }
}

// Resolve in an UI-Router state
resolve: foo
```

## Components

### IIFE

All components are located in an IIFE with strict mode enabled:

```js
(function () {
    'use strict';

    // Code
})();
```

### Constants

To be used to extract reusable groups of values instead of hardcoding them inside a component, e.g.

```js
angular.module('baz', [])
    .constant('FOO', 'bar');
```

If they have a sufficient complexity or are plain large, you should think about extracting them to a preceding variable:

```js
var FOO = {
    bar: {
        baz: 'baz'
    }
};

angular.module('baz', [])
    .constant('FOO', FOO);
```

### Component

Components are simplified attributue directives with sensible defaults. They are implemented as objects and to be preferred since they enable a more componentized appraoch to development. Here are the defaults:

1. `restrict` = `E`
2. `scope` = `true`
3. `controllerAs` = `$ctrl`
4. `bindToController` = `true`

They are to be inlined according the following pattern:

```js
angular
    .module('foo', [])
    .component('bar', {
        bindings: {
            baz: '@'
        },
        templateUrl: 'app/foo/bar/baz.html'
        controllerAs: 'vm'
    });
```

Even if there's no explicit controller function, you should always set it to `vm` to be consistent.

### Directives

#### Element Directives

Use `component`.

#### Attribute Directives

1. Used for DOM-manipulations and validation
2. The parameters of the link function are always named `scope`, `elem`, `attrs`, `ctrl` or `ctrls` if you have an array.

    ```js
    angular
        .module('foo.bar.ui', [])
        .directive('fooBar', fooBar)

    function fooBar() {
        return {
            restrict: 'A',
            require: 'ngModel',
            link:  function link(scope, elem, attrs, ctrl) {
                // ...
            }
        };
    }
    ```

### Filter

Use sparingly since they are very costly.

### Controller

1. Always use the `Controller` suffix
2. Implemented in a standalone function
3. Minimal amount of logic
4. Logic goes to the service to be testable
5. No DOM-manipulations
6. Try to get required data inside the `resolve` of the state
7. Always bind `this` to `vm` and don't use `$scope`
8. Use `extend` blocks to create meaningful semantic blocks

```js
function FooController() {
    var vm = this;

    _.extend(vm, {
        // Variables
    });

    _.extend(vm, {
        // Functions
    });

    _.extend(vm, {
        // Another semantic block
    });
}
```

### Services

1. Don't use technical suffixes
2. Don't use `factory` the difference is confusing and `service` is more flexible

#### Service Pattern

This is known as the Revealing Module Pattern and allows for a clean, testable API.

```js
function foo() {
    return {
        bar: bar
    };

    var baz = 'baz';

    function bar() {
        // ...
    }
}
```

#### Resource Model Pattern

Represents the model layer in the frontend and allows to have class-like constructs for REST and specifcally HAL resources.

```js
angular
    .module(foo.bar, [])
    .service('Bar', Bar);

function Bar(
    // Dependencies
) {
    function Bar(
        // Parameters
        resource
    ) {
        // Instance
        var bar = _.extend({
            // Variables
        }, resource);

        _.extend(bar, {
            // Instance methods
        });

        return bar;
    }
    _.extend(Bar, {
        // Static class methods
    });

    return Bar;
}
```

An concrete example:

```js
angular
    .module(ak.customer, [])
    .constant('CUSTOMER_URL', 'api/v1/customer')
    .service('Customer', Customer);

function Customer(
    CUSTOMER_URL,
    restClient
) {
    function Customer(
        resource
    ) {
        var customer = _.extend({
            fullname: resource.firstname + ' ' + resource.lastname
        }, resource);

        _.extend(Customer, {
            delete: function () {
                return restClient.delete(resource._links['delete'])
            }
        });

        return customer;
    }
    _.extend(Customer, {
        getAll: function () {
            return restClient.get(CUSTOMER_URL)
                .then(function(customers) {
                    return _.map(customers, function (customer) {
                        return new Customer(customer);
                    });
                });
        }
    });

    return Customer;
}
```

And then you could do things like:

```js
Customer.getAll()
    .then(function (customers) {
        vm.customers = customers;
    });

_.first(vm.customers).delete();
```

## Promises

1. Always `return` the promise for chaining
2. Extract functions if it could add clarity

```js
return promise
    .then(foo1)
    .then(foo2)
    .then(foo3)
    .catch(bar)
    .finally(baz);
```

## Scope, Watches, Emits, Broadcasts

Very often you don't need it. Ask yourself twice why you injected it and if you can solve it via bindings.

## Wrapper

Use the Angular wrappers instead of the native ones.

* `$timeout` instead `setTimeout`
* `$interval` instead `setInterval`
* `$window` instead `window`
* `$document` instead `document`

## Third-party libraries

It is advisable to wrap third-party libraries in a service if you need to modify them and delete them from global namespace in order to use Angular's dependency injection, e.g.

```js
angular
    .module('moment',[])
    .service('moment', moment);

function ($window) {
  var moment = $window.moment;

  _.extend(moment, {
      foo: /...
  });

  try {
    delete $window.moment;
  } catch (e) {
      $window.moment = undefined;
      return moment;
    }
}
```

## Software Engineering Principles

* [SRP](https://en.wikipedia.org/wiki/Single_responsibility_principle)
* [SOC](https://en.wikipedia.org/wiki/Separation_of_concerns)
* [YAGNI](http://en.wikipedia.org/wiki/You_Ain%27t_Gonna_Need_It)
* [DRY](http://en.wikipedia.org/wiki/Don%27t_repeat_yourself)
* [KISS](http://en.wikipedia.org/wiki/KISS_principle)
