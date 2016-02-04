# Angular

## Naming

### Folder structure

Prefer a flat feature-based structure:

```
account/
    account.js
    account.ui.js
    account.html
```

Do not use functional-based structure:

```
services/
    account.js
```

Try to avoid generic trash folders:

```
common/
utility/
```

### Components

Directives and Filter: `prefixName` in `name.ui.js`
States: `prefix.name` in `name.ui.js`
Controller: `NameController`
Services: `name.js`

## Databinding

Prefer `ngBind` over `{{}}`

```html
<!-- Avoid -->
<span>{{::vm.foo}}</span

<!-- Prefer -->
<span ng-bind="vm.foo"></span>
```

## Modules

Use chaining:

```js
angular
    .module('foo', [])
    .directive('fooBar', fooBar)
    .controller('BarController', BarController)
    .filter('fooBaz', fooBaz)
    .constant('FOOBARBAZ' FOOBARBAZ);
```

## Constants

1. Uppercase
2. Defined at the beginning

```js
var FOO = 'bar';

angular.module('baz', [])
    .constant('FOO', FOO);
```

## Directives

### Component Directives

1. Always prefix
2. Always restrict to `E` or `A`
3. Always set an isolate scope
5. Register controller on the module
6. The short name is always `vm`
5. Bind parameters with `bindToController`

```js
angular
    .module('foo.bar.ui', [])
    .directive('fooBar', fooBar)
    .controller('BarController', BarController);

function fooBar() {
    return {
        restrict: 'E',
        templateUrl: 'foo/bar/baz.html',
        scope: true,
        controller: 'BarController',
        controllerAs: 'vm',
        bindToController: {
            foo: '@?',
            bar: '=*',
            baz: '&'
        }
    };
}

function BarController() {
    // ...
}
```

### Validation Directives

1. Separate link function
2. The names of the parameters are always `scope`, `elem`, `attrs`, `ctrl` or `ctrls` if an array is required.

```js
angular
    .module('foo.bar.ui', [])
    .directive('fooBar', fooBar)

function fooBar() {
    return {
        restrict: 'A',
        require: 'ngModel',
        link: link
    };
}

function link(scope, elem, attrs, ctrl) {
    // ...
}
```

## Controller

1. Always suffix with `Controller`
2. Always write in separate function
3. Minimize logic
4. Move asynchronous calls to a service
5. Try to resolve asynchronous data in the state
3. Always bind `this` to `vm`

```js
function FooController() {
    var vm = this;

    angular.extend(vm, {
        // Variables
    });

    angular.extend(vm, {
        // Functions
    });
}
```

## Service/Factory

1. Avoid technical suffixes.
2. Services have lowercase names.
3. Factories have uppercase names.
4. Use services if no resource model is required.

### Service Pattern

```js
function myService() {
    return {
        foo: foo
    };

    var bar = 'bar';

    function foo() {
        // ...
    }
}
```

### Resource Model Pattern

Represents the model layer and allows to map HAL resources. It emulates object-oriented programming by allowing for Constructors and creating instances with the `new` keyword.

```js
angular
    .module(foo.bar, [])
    .factory('Bar', Bar);

function Bar(
    // Dependencies
) {
    function Bar(
        // Parameters
        resource
    ) {
        // Instance
        var bar = angular.extend({
            // Variables
        }, resource);

        angular.extend(bar, {
            // Instance methods
        });

        return bar;
    }
    angular.extend(Bar, {
        // Static class functions
    });

    return Bar;
}
```

## Promises

1. Always return promises for chaining
2. Extract functions
3. Always use shorthand notation for function passing

```js
return promise
    .then(foo1)
    .then(foo2)
    .then(foo3)
    .catch(bar)
    .finally(baz);
```
