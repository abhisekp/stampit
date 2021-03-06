<img src="https://raw.githubusercontent.com/stampit-org/stampit-logo/master/stampit-logo.png" alt="stampit" width="160" />

# Stampit [![Travis-CI](https://travis-ci.org/stampit-org/stampit.svg)](https://travis-ci.org/stampit-org/stampit) [![Gitter](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/stampit-org/stampit?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge) [![Twitter Follow](https://img.shields.io/twitter/follow/stampit.svg?style=social&label=Follow&maxAge=2592000)](https://twitter.com/stampit_org)

**Create objects from reusable, composable behaviors** 
 
Stamps are [standardised](https://github.com/stampit-org/stamp-specification) composable factory functions. **Stampit** is an [infected compose](https://medium.com/@koresar/fun-with-stamps-episode-8-tracking-and-overriding-composition-573aa85ba622) featuring friendly handy API.

 
## Simplest Example

```js
const MyStamp = stampit()       // create new empty stamp
.props({                        // add properties to your future objects
  myProp: 'default value'
})
.methods({                      // add methods to your future objects
  getMyProp() {
    return this.myProp;
  }
})
.init(function ({value}) {      // add initializers to your future objects
  this.myProp = value || this.myProp;
})
.compose(AnotherStamp);         // add other stamp behaviours to your objects

console.log(typeof MyStamp);                   // 'function'
console.log(MyStamp());                        // { myProp: 'default value' }

console.log(typeof MyStamp().getMyProp);       // 'function'
console.log(MyStamp().getMyProp());            // default value

console.log(MyStamp('new value'));             // { myProp: 'new value' }
console.log(MyStamp('new value').getMyProp()); // new value
```

Stampit uses [three different kinds of prototypal OO](https://vimeo.com/69255635) to let you inherit behavior in a way that is much more powerful and flexible than classical OO.


## Status

* **v1**. `npm i stampit@1`
* **v2**. `npm i stampit@2` [Breaking changes](https://github.com/stampit-org/stampit/releases/tag/2.0)
* **v3**. `npm i stampit` [Breaking changes](https://github.com/stampit-org/stampit/releases/tag/3.0). Compatible with the [stamp specification](https://github.com/stampit-org/stamp-specification)


## Install

[![NPM](https://nodei.co/npm/stampit.png)](https://www.npmjs.com/package/stampit)

Browsers: [![CDNJS](https://img.shields.io/badge/cdnjs.com--green.svg)](https://cdnjs.com/libraries/stampit)
[![UNPKG](https://img.shields.io/badge/unpkg.com--green.svg)](https://unpkg.com/stampit@latest/dist/stampit.umd.min.js)

**WARNING!** If running in node.js <= v0.12 or IE <= 11 then you'd need to polyfill the `Object.assign`. Like [this](https://github.com/sindresorhus/object-assign), or [that](https://github.com/Financial-Times/polyfill-service/blob/master/polyfills/Object/assign/polyfill.js) ([autodetected](https://cdn.polyfill.io/v2/polyfill.min.js?features=Object.assign)).

## API

See the [API](docs/API.md).


# What's the Point?

Prototypal OO is great, and JavaScript's capabilities give us some really powerful tools to explore it, but it could be easier to use.

Basic questions like "how do I inherit privileged methods and private data?" and 
"what are some good alternatives to inheritance hierarchies?" are stumpers for many JavaScript users.

Let's answer both of these questions at the same time.

```js
// Some privileged methods with some private data.
const Availability = stampit().init(function() {
  var isOpen = false; // private

  this.open = function open() {
    isOpen = true;
    return this;
  };
  this.close = function close() {
    isOpen = false;
    return this;
  };
  this.isOpen = function isOpenMethod() {
    return isOpen;
  }
});

// Here's a stamp with public methods, and some state:
const Membership = stampit({
  methods: {
    add(member) {
      this.members[member.name] = member;
      return this;
    },
    getMember(name) {
      return this.members[name];
    }
  },
  properties: {
    members: {}
  }
});

// Let's set some defaults:
const Defaults = stampit({
  init({name, specials}) {
    this.name = name || this.name;
    this.specials = specials || this.specials;
  },
  properties: {
    name: 'The Saloon',
    specials: 'Whisky, Gin, Tequila'
  }
});

// Classical inheritance has nothing on this.
// No parent/child coupling. No deep inheritance hierarchies.
// Just good, clean code reusability.
const Bar = stampit(Defaults, Availability, Membership);

// Create an object instance
const myBar = Bar({name: 'Moe\'s'});

// Silly, but proves that everything is as it should be.
myBar.add({name: 'Homer'}).open().getMember('Homer');
```

For more examples see the [API](docs/API.md).


# More info

Stampit was written as an example for the book, ["Programming JavaScript Applications" (O'Reilly)](http://pjabook.com). See [this page](https://github.com/stampit-org/stampit/blob/master/docs/pjabook-updated-examples.md) to make book examples work with the latest stampit.

Looking for a deep dive into prototypal OO, stamps, and the Two Pillars of JavaScript? [Learn JavaScript with Eric Elliott](https://ericelliottjs.com).

**React Users.** Stampit *loves* React. Check out [react-stamp](https://github.com/stampit-org/react-stamp) for composable components.

