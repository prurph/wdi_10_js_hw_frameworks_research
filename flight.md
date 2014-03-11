<!-- How is your chosen framework similar to Angular, and how is it different? What kind of templates does it use? Are there any options to easily integrate it with Rails? Are there any notable apps built with it? -->
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
[Backbone.js router](http://backbonejs.org/). Overall if you need really robus
MV-x functionality, it might be better to look elsewhere.

### What's a component?

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

