# Flight.js

### Flight's alright.
Flight is a lightweight JS framework that defines "components" and maps them to
DOM nodes. It maps its functionality directly to DOM nodes, not unlike how
Angular uses DOM-based templating. Rather than communicating directly,
components cannot be referenced by other components, nor do they become global
properties. Instead Flight's components use plain old HTML DOM actions,
subscribed to by other components, to communicate with one another.

Flight's creators boast that this makes components "highly portable and easily
testable", since each is completely independent of one another. Testing support
is offered for both Jasmine and Mocha, and Flight additionally offers mixins
to extend the functionality of existing components.

The framework strives to impose very little structure, and in fact Flight does
not offer a router. In this sense Flight is not a "full" framework, rather it
allows incorporation of additional components as necessary. External routers
that have been used with Flight include: [Flatiron
Director](https://github.com/flatiron/director),
[Crossroads](http://millermedeiros.github.io/crossroads.js/) and the
[Backbone.js router](http://backbonejs.org/). Overall if you need fully robust
MV-x functionality, it might be better to look elsewhere.

### What's a component?
A component is just a JS constructor with properties mixed into its prototype.
All Flight components have basic functionality like event handling, and custom
properties which describe its behavior. You can think of this as a model in a
framework such as Angular, but unlike Angular, a Flight component cannot be
referenced directly. It should not be confused with a factory or service in
Angular since these are singletons, whereas instances of components are created
by binding Flight components to DOM nodes (more on this shortly).

Writing a component is straightforward. The basic steps are:

-  Create a module using the `define` function that requires Flight's component
module: `lib/component` The component module makes a function available,
typically called `defineComponent`
-  `defineComponent` accepts mixins/functions
and returns a new component constructor with these mixins applied to its
prototype.
-  Use `after()` and pass the name of a function (typically
`initialize`) and a callback with event handlers to bind to the component when
it is initialzed by binding it to a DOM element

This is how this looks in practice:

    `define(function (require) {
      var defineComponent = require('flight/lib/component');

      // We then pass mixins or single functions to defineComponent
      return defineComponent(componentName)

      function componentName() {
        this.doStuff = function() {
          // stuff
        }
        this.doMore = function() {
          // more stuff
        }

        // and here are some handlers to bind after initializing the component
        // after takes two parameters, a function name and a function to be
        // executed on completion of the function named by the first parameter
        this.after('initialize', function() {
          this.on('click', this.doStuff);
          this.on('mouseover', this.doMore);
        });
      }
    })

Note that the binding of functions to events is both the essence of Flight and
very straightforward since it is functionally identical to vanilla JavaScript.

Attaching a component to a node effectively creates an instance of that
component. This is again done with `define`, assigning a variable to the result
of `require` passed the component constructor and then attached to the DOM using
`attachTo`. The argument to `attachTo` can be a DOM node, jQuery object, or CSS
selector.

    `define(function (require) {
        var Component = require('componentName');

        // attachTo takes a selector and an optional options object
        Component.attachTo('#main', {
          'nextPage': '#nextPage'
          'prevPage': '#prevPage'
          });
      });

Note that this is quite different from Angular, where directives can be embedded
directly into the HTML. With Flight, we use the attachTo method to sitck
components to things, at which point the component's functionality is completely
set.

Finally, once attached, component instances access their node object via `node`
or the jQuery version `$node`.

### Rails takes Flight
[FlightForRails](https://github.com/rezwyi/flight-for-rails) is a plugin that
allows a Rails application to integrate with Flight. It's as easy as including
`gem 'flight-for-rails'`, running `bundle install` and requiring it in your
`application.js`. Since you will most likely be defining files in
`app/assets/javascript/components/component-name.js[.coffee]`, be sure to also
require these in your manifest file as:

    `//= require_directory ./components`

As you can guess from the .coffee above--if you're using Flight in Rails you're
free to write in CoffeeScript!

### Famous(?) Flights
The highest-profile example of Flight usage in the wild is [TweetDeck](https://about.twitter.com/products/tweetdeck). Although not built from the ground up
with Flight, TweetDeck has since switched over to Flight. Time will tell if this
relatively new framework--wait for it--takes flight!
