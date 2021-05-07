# Partial Expression Syntax for ECMAScript
This proposal introduces a new syntax using the `#`, `?` (any number of ?s after each other), `?x` (x ∈ ℕ), `?r` tokens which allow you to partially apply an expression by acting as placeholders for a value or values.

## Status

**Stage:** [#1](https://github.com/trustedtomato/proposal-partial-expression/issues/1)  
**Champion:** [#1](https://github.com/trustedtomato/proposal-partial-expression/issues/1)

Any and all feedback and ideas are greatly appreciated. Use the issues to post questions/ideas and send pull requests for any content updates. Fixes, clarifications, and especially usage examples are definitely helpful.

_For more information see the [TC39 proposal process](https://tc39.github.io/process-document/)._

## Authors

* Halasi Tamás ([@trustedtomato][Tomato])

## Some examples which regularly come up
```javascript
[1,2,3,4,5,6,7,8,9].map(#1+?); // partially apply an operator
//=> [2,3,4,5,6,7,8,9,10]

[1,2,3,4,5,6,7,8,9].reduce(#?+?); // use an operator as a function
//=> 45

['cat', 'coconut', 'carrot', 'dog', 'sun'].filter(#?.startsWith('c')); // partially apply a function/method
//=> ['cat', 'coconut', 'carrot'] 
```

# Proposal
## `#` and `?`
The `#` operator (precedence: [3.5](https://developer.mozilla.org/hu/docs/Web/JavaScript/Reference/Operators/Operator_Precedence)) makes the affected expression a function. All upcoming tokens are only interpreted in this expression.

The `?` is a variable which will have the value of the corresponding argument. The first one stands for the first argument, the second for the second, etc. The order is determined by the order of their appearance in the code.

```javascript
const add = #?+?;
// const add = (x,y) => x+y;

const addOne = #1+?;
// const addOne = (x) => 1+x;
```

## `?x` (`?0, ?1, ?2`)
The number specifies the ordinal position of the parameter. They allow the usage of an argument more than once & swap their order. Numbering starts from 0. The unnumbered `?`s fill out the left out parameters from left to right.

```javascript
const power = #?0**?1;
// const power = (x,y) => x**y;

const basicTetration = #?0**?0;
// const basicTetration = x => x**x;

const flippedPower = #?1**?0;
// const flippedPower = (x,y) => y**x;

const foo = #bar(?1,?2,?,?1,?,?4,...?r,?);
// const foo = (x0,x1,x2,x3,x4,x5,...xs) => bar(x1,x2,x0,x1,x3,x4,...xs,x5);
```

## `?r`
The `?r` is an array of the arguments after the last used parameter.

```javascript
const maxWithTheMinOf0 = #Math.max(0, ?, ...?r, ?);
// const maxWithTheMinOf0 = (x,y,...zs) => Math.max(x,...zs,y)

const foo = #Math.max(0, ?, ...?r, ?2);
// const maxWithTheMinOf0 = (x,_,y,...zs) => Math.max(x,...zs,y)
```


# Parsing

Conditional expressions:  
The `?` token is used in conditional expressions, but its not ambigous, because in this proposal, a `?` cannot follow an expression, while in a conditional expression it always does. (e.g. in `f(a?` the `?` is definitely a conditional operator while in `f(?` it's definitely a placeholder).

[Optional chaining](https://github.com/TC39/proposal-optional-chaining):  
The operator `?.` must appear directly after an expression, while the `?` variable can't.


# TODO

The following is a high-level list of tasks to progress through each stage of the [TC39 proposal process](https://tc39.github.io/process-document/):

### Stage 1 Entrance Criteria

* [ ] Identified a "[champion][Champion]" who will advance the addition.  
* [x] [Prose][Prose] outlining the problem or need and the general shape of a solution.  
* [x] Illustrative [examples][Examples] of usage.  
* [x] ~High-level API~ _(proposal does not introduce an API)_.  

### Stage 2 Entrance Criteria

* [ ] [Initial specification text][Specification].  
* [ ] _Optional_. [Transpiler support][Transpiler].  

### Stage 3 Entrance Criteria

* [ ] [Complete specification text][Specification].  
* [ ] Designated reviewers have [signed off][Stage3ReviewerSignOff] on the current spec text.  
* [ ] The ECMAScript editor has [signed off][Stage3EditorSignOff] on the current spec text.  

### Stage 4 Entrance Criteria

* [ ] [Test262](https://github.com/tc39/test262) acceptance tests have been written for mainline usage scenarios and [merged][Test262PullRequest].  
* [ ] Two compatible implementations which pass the acceptance tests: [\[1\]][Implementation1], [\[2\]][Implementation2].  
* [ ] A [pull request][Ecma262PullRequest] has been sent to tc39/ecma262 with the integrated spec text.  
* [ ] The ECMAScript editor has signed off on the [pull request][Ecma262PullRequest].

# Related proposals
It would play nicely with the [pipeline operator](https://github.com/tc39/proposal-pipeline-operator).  
Or you might be interested in other partial application proposals:
- a `.papp` method for functions: [gilbert/es-papp](https://github.com/gilbert/es-papp)
- a proposal very similar to this, but just for functions: [rbuckton/proposal-partial-application](https://github.com/rbuckton/proposal-partial-application)

[Tomato]: https://github.com/trustedtomato
[Champion]: #todo
[Prose]: #proposal
[Examples]: #examples
[Specification]: #todo
[Transpiler]: #todo
[Stage3ReviewerSignOff]: #todo
[Stage3EditorSignOff]: #todo
[Test262PullRequest]: #todo
[Implementation1]: #todo
[Implementation2]: #todo
[Ecma262PullRequest]: #todo
