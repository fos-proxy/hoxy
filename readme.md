This is a fork of https://github.com/greim/hoxy 

# Hoxy

Hoxy is an HTTP traffic-sniffing and manipulation tool for JavaScript programmers.

## Full Documentation

http://greim.github.io/hoxy/

## Example

```js
const hoxy = require('hoxy');
const proxy = hoxy.createServer().listen(8080);
proxy.intercept({

  // intercept during the response phase
  phase: 'response',

  // only intercept html pages
  mimeType: 'text/html',

  // expose the response body as a cheerio object
  // (cheerio is a jQuery clone)
  as: '$'
}, function(req, resp) {

  resp.$('title').text('Unicorns!');
  // all page titles will now say "Unicorns!"
});
```

## Version 3.0

Hoxy has released version 3.0.
This release simplifies the API and better supports ES6.
Notable changes:

 * A `done` callback is no longer passed as the third arg to interceptors. Interceptor arity is, accordingly, no longer a switch for async behavior. Rather, it solely depends on the return type of the interceptor (i.e. promises or iterators over promises).
 * The third argument to interceptors is now the `cycle` object, === to `this`. This was based on a suggestion from [@nerdbeere](https://github.com/nerdbeere), with a view toward supporting arrow functions, in which `this` is lexical.
 * The CLI has been completely removed from the project. The reasoning is that, by simplifying the project, I can more easily maintain it. If there's a need, it can be brought back as a separate npm module. Perhaps somebody else can take that on.
 * Undocumented `hoxy.forever()` function goes away.


# Regarding Development of this Tool

I began writing code for this in 2010 while I worked at Cisco—and later at various startups—to solve my own needs for advanced testing and debugging production websites. The most common—although by no means only—use case for me was swapping in my own JavaScript onto web pages, in order to do rapid debugging without actually pushing console.logs to a staging server or (god forbid!) production. I could just edit a file on my local disk and reload my browser.

This enabled me to act quickly when hard-to-debug production issues arose, which translated into a competitive advantage for both myself and my company. Over time, however, I've used it less frequently, for a few reasons:

 1. **Devtools.** Built-in debuggers got *wayyy* more powerful. Starting in ~2014, major browsers shipped with source-map-enabled debuggers, which allow setting breakpoints and stepping through code in original, non-uglified, non-bundled form. That was key, and alleviated 80% of what I need Hoxy for.
 1. **HTTPS everywhere.** HTTPS became widely used in production during this same period. Hoxy supports HTTPS proxying, but involves extra setup, which adds friction to an already high-friction tool. Seriously, Hoxy makes it easier, but MITM-hacking your own HTTP traffic isn't exactly the definition of ease and simplicity.
 1. **Evergreen browsers.** Happily, the amount of time I spend on cross-browser differences has gone down as of mid 2018. Along those lines, a secondary use for Hoxy was injecting code into older, single-digit versions of IE, which had zero devtools capabilities. Nowadays I simply don't bother with those browsers.
 1. **Functional programming.** React (which I started using in 2014) is an enabling technology for functional programming. FP in turn makes it easier to test, debug, and reason about a piece of code in isolation. This even further alleviates the need to use Hoxy to see how something behaves in prod, since I can just write unit tests, reason about the code in my head, etc.

All of that said, this is still a valuable tool in my toolbox, and I use it when the need arises. I'm not putting much time and attention into bug fixes and feature improvements these days, but PRs are always welcome!
