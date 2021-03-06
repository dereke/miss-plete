# MissPlete

MissPlete is a misspelling-tolerant autocomplete written in ECMAScript 2015, aka
ECMAScript 6 (ES6).

It supports synonyms and it can be customized with any algorithm to select and
sort the completions. By default it uses a
[Jaro–Winkler distance](https://en.wikipedia.org/wiki/Jaro%E2%80%93Winkler_distance)
algorithm, which allows for
[better sloppy interaction](https://vimeo.com/28758945#t=11m35s)
than the usual completion based on exact substring matches.

Less than 220 lines of code. No dependencies.

# Demo

http://xavi.github.io/miss-plete

# Usage

Because
[ES6 is not yet fully supported in any browser](https://kangax.github.io/compat-table/es6/),
to use MissPlete you'll have to transpile its files (MissPlete.js,
jaroWinkler.js, and memoize.js) to ES5 and bundle them with your project (using,
for example, [Babel](https://babeljs.io/) and
[webpack](https://webpack.github.io/)).
 
Include the bundled JavaScript and miss-plete.css on the project page.

This sample code shows how to use it:

```javascript
import MissPlete from './MissPlete.js';

new MissPlete({
  input: document.querySelector('input[name="city"]'),

  // Each subarray contains an option and all its synonyms
  options: [["Barcelona", "BCN"], ["San Francisco", "SF"]],

  // OPTIONAL
  // It must return an object with at least the properties `score` and  
  // `displayValue`.
  // Default is a Jaro–Winkler similarity function.
  scoreFn: (inputValue, optionSynonyms) => {
    // Crazy random example
    const score = Math.random();
    return { score: score, displayValue: `${optionSynonyms[0]} (${score})` };
  },

  // OPTIONAL
  // Called for each scored option, in order, starting with the one with the
  // greatest score. It's passed the scored option (as returned by scoreFn)
  // and its index in the score-sorted list. It must return the <li> node
  // to display, or null if nothing else has to be displayed.
  // Default returns <li> nodes for the 8 best-scored options.
  listItemFn: (scoredOption, itemIndex) => {
    const li = scoredOption.score < 0.5 ? null : document.createElement("li");
    li && li.appendChild(document.createTextNode(scoredOption.displayValue));
    return li;
  }  
});
```


## License

Copyright © 2015 Xavi Caballé

Licensed under the [MIT License](LICENSE).
