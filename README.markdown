Meteor Reactive + Filtering Routers
-----------------------------------


A `ReactiveRouter` is a simple beast, it's just a Bootstrap Router with a reactive variable `current_page()`. To set the variable, call `goto()`.

```js
MyRouter = ReactiveRouter.extend({
  routes: {'': 'home'},
  home: function() { this.goto('home'); }
})

MyRouter.current_page(); // might be 'home'
```

A `FilteredRouter` adds filtering capabilities to the string returned by `current_page`. For instance, you could hook into the new Meteor Auth[https://github.com/meteor/meteor/wiki/Getting-started-with-Auth] stuff like so:

```js
MyRouter = FilteredRouter.extend({
  initialize: function() {
    FilteredRouter.prototype.initialize.call(this);
    this.filter(this.require_login, {only: home});
  },
  
  require_login: function(page) {
    if (Meteor.user()) {
      return page;
    } else {
      return 'signin';
    }
  },
  
  routes: {'': 'home'},
  home: function() { this.goto('home'); }
})

MyRouter.current_page(); // might be 'home'
```
