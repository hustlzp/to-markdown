# to-markdown

An HTML to Markdown converter written in JavaScript.

The API is as follows:

    toMarkdown(stringOfHTML, options);

## Installation

### Browser

Download the compiled script located at `dist/to-markdown.js`.

    <script src="PATH/TO/to-markdown.js"></script>

    <script>toMarkdown('<h1>Hello world!</h1>')</script>

Or with **Bower**:

    $ bower install to-markdown

    <script src="PATH/TO/bower_components/to-markdown/dist/to-markdown.js"></script>

    <script>toMarkdown('<h1>Hello world!</h1>')</script>

### Node.js

    $ npm install to-markdown

    var toMarkdown = require('to-markdown');
    toMarkdown('<h1>Hello world!</h1>');

(Note it is no longer necessary to call `.toMarkdown` on the required module as of v1.)

## Options

### `converters` (array)

to-markdown can be extended by passing in an array of converters to the options object:

    toMarkdown(stringOfHTML, { converters: [converter1, converter2, …] });

A converter object consists of a **filter**, and a **replacement**. This example from the source replaces `code` elements:

    {
      filter: 'code',
      replacement: function(innerHTML) {
        return '`' + innerHTML + '`';
      }
    }

#### `filter` (string|array|function)

The filter property determines whether or not an element should be replaced. DOM nodes can be selected simply by filtering by tag name, with strings or with arrays of strings:

* `filter: 'p'` will select `p` elements
* `filter: ['em', 'i']` will select `em` or `i` elements

Alternatively, the filter can be a function that returns a boolean depending on whether a given node should be replaced. The function is passed a DOM node as its only argument. For example, the following will match any `span` element with an `italic` font style:

    filter: function (node) {
      return node.tagName === 'SPAN' && /italic/i.test(node.style.fontStyle);
    }

#### `replacement` (function)

The replacement function determines how an element should be converted. It should return the markdown string for a given node. The function is passed the node’s innerHTML[1], as well as the node itself (used in more complex conversions).

The following converter replaces heading elements (`h1`-`h6`):

    {
      filter: ['h1', 'h2', 'h3', 'h4', 'h5', 'h6'],

      replacement: function(innerHTML, node) {
        var hLevel = node.tagName.charAt(1);
        var hPrefix = '';
        for(var i = 0; i < hLevel; i++) {
          hPrefix += '#';
        }
        return '\n' + hPrefix + ' ' + innerHTML + '\n\n';
      }
    }

[1] The innerHTML parameter has had leading/trailing whitespace removed, and has HTML entities decoded.

## Tests

to-markdown uses QUnit for testing. To run the tests in the browser, first make sure you have node.js/npm installed, then:

    $ npm install --dev
    $ bower install --dev

Then open `test/test-runner.html`.

To run in node.js:

    $ npm install --dev
    $ npm test

## Licence

to-markdown is copyright &copy; 2011-15 [Dom Christie](http://domchristie.co.uk) and released under the MIT license.