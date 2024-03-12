# tdomjs
## Client and server side dom template engine

Build dom structure easy way with plain JavaScript. Can be used on both client
and server side.

### Installation

	$ npm install tdomjs

## Usage

What would be the easiest, most intuitive way to build html5 DOM tree with plain
JavaScript ?

```javascript
var mytemplate = function () {
  header(
    h1('Heading'),
    h2('Subheading'));

  nav(
    ul({ 'class': 'breadcrumbs' },
      li(a({ href: '/' }, 'Home')),
      li(a({ href: '/section/'}, 'Section')),
      li(a('Subject'))));

  article(
    p('Lorem ipsum...'));

  footer('Footer stuff');
};
```

This is how templates for tdomjs can be written.

Plain `tdomjs` usage example:

```javascript
var tdomjs = require('tdomjs')(document);

var ns = tdomjs.ns;
var dom = tdomjs.collect(function () {
  ns.header(
    ns.h1('Heading'),
    ns.h2('Subheading'));

  ns.nav(
    ns.ul({ 'class': 'breadcrumbs' },
      ns.li(a({ href: '/' }, 'Home')),
      ns.li(a({ href: '/section/'}, 'Section')),
      ns.li(a('Subject'))));

  ns.article(
    ns.p('Lorem ipsum...'));

  ns.footer('Footer stuff');
});

document.body.appendChild(dom); // Insert generated DOM into document body
```

To use tdomjs functions literally as in first example, you will need to prepare dedicated function wrapper
(either programmatically or manually) as e.g. following:

```javascript
var myTemplate = (function () {}
  var { article, footer, h1, h2, header, li, nav, p, ul } = ns;
  return function () {
    header(
      h1('Heading'),
      h2('Subheading'));

    nav(
      ul({ 'class': 'breadcrumbs' },
        li(a({ href: '/' }, 'Home')),
        li(a({ href: '/section/'}, 'Section')),
        li(a('Subject'))));

    article(
     p('Lorem ipsum...'));

    footer('Footer stuff');
  };
}());
var dom = tdomjs.collect(myTemplate);
```

### Other notes

You can create custom elements:

```javascript
var myCustomElement = tdomjs.ns.myCustomElement = tdomjs.ns.element.bind(tdomjs, 'my-custom-element');
```

Optionally you may also provide some custom constructor or methods for that element:

```javascript
// This has to be defined before any `my-custom-element` is created by tdomjs
require('tdomjs/ext')['my-custom-element'] = {
  _construct: function (/* constuctor args */) {},
  methodA: function () { ... }
};
```

You can save references to elements and operate on them later:

```javascript
var myul = ul(li('one'), li('two'), li('three'));

// ... some code ...

// add extra items to myul
myul(li('four'), li('five'));

// append myul into other element
div(myul);
```

You can access DOM elements directly, just invoke returned function with no
arguments

```javascript
(myul() instanceof DOMElement) === true
```

Comment type nodes:

```javascript
comment('my comment');
```

CDATA type nodes

```javascript
cdata('cdata text');
```

Text nodes in main scope:

```javascript
text('my text');
```

Elements with names that are reserved keywords in JavaScript language, like
'var', should be created with preceding underscore added to its name:

```javascript
_var('var content');
```

	$ npm test
