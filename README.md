[![Build Status](https://travis-ci.org/the-control-group/scim-query-filter-parser-js.svg?branch=master)](https://travis-ci.org/the-control-group/scim-query-filter-parser-js)

tl;dr
-----

```js
    const Filter = require('scim-query-filter-parser');

    const filter = new Filter('userName eq "somebody123"');

    // get the parsed filter as an RPN stack
    const rpn = filter.rpn;

    // get the parsed filter as an expression tree
    const tree = filter.tree;

    // get the parsed filter as an expression tree
    const results = [
        {userName: 'somebody123'},
        {userName: 'somebody456'}
    ].filter(filter.test);
```

Description
-----------

This implements a parser for the filtering syntax [defined in  System for Cross-Domain Identity Management (SCIM) Protocol 1.1](http://www.simplecloud.info/specs/draft-scim-api-01.html#rfc.section.3.2.2.1). It is originally a port of [scim-query-filter-parser-rb](https://github.com/ingydotnet/scim-query-filter-parser-rb) and was built for use by [AuthX](https://github.com/the-control-group/authx);

This package can parse one of these filter queries and produce a [Reverse Polish Notation (RPN)](https://en.wikipedia.org/wiki/Reverse_Polish_notation) stack or expression tree.

It can also be used to test resources against the parsed filter query.

For example, parse this filter query string:

```
userType eq "Employee" and (emails co "example.com" or emails co "example.org")
```

Into this RPN stack (array):

```js
['userType', '"Employee"', 'eq', 'emails', '"example.com"', 'co', 'emails', '"example.org"', 'co', 'or', 'and']
```

Or, optionally into this expression tree:

```js
['and',
    ['eq',
        'userType',
        '"Employee"'],
    ['or',
        ['co',
            'emails',
            '"example.com"'],
        ['co',
            'emails',
            '"example.org"']]]


```

Methods & Properties
--------------------

### `new Filter(filterQueryString, [preserveCase=false])`
Parse a SCIM filter query and return a `Filter` object.

### `filter.rpn`
Get the RPN array created by the most recent parse.

### `filter.tree`
Get the parse result converted to a tree form.

### `filter.test(resource)`
Tests a resource against a filter. This method is already bound to the filter, so that it can easily be used as an argument for `Array.prototype.filter` and similar methods.
