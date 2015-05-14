# Prototypal Objects vs Object Literals

## TL;DR;

- Prototypal inheritance outperforms object literal alternatives
- 32x memory savings in Chrome
- 6x faster in Chrome
- FF yields similar ratios

## Speed

I measured the performance of a variety of object construction alternatives, as
seen here:

- http://jsperf.com/prototype-vs-object-literals/2

- Prototypal is at least 2x faster in Chrome and Safari than literal
- The results were much more even in FF than in Chrome and Safari.

A variety of factors might affect this, including the contrived constructors
I created. However, these results are consistent with past comparisons I've made
in real-world scenarios, so I believe them to be accurate.

Also of note, a third alternative which also performed really well was to 
essentially ignore OO altogether and just pass data (as object-literal records) 
through simple functions. More on this in the conclusion.

Ultimately, though, all options perform fairly well on my souped up Mac (1M-8M 
  operations per second). Things weren't so chummy on my underpowered Android 
  (100K-600K operations per second).
  
## Memory

Using this as an object literal constructor method:

```
function MakeObj(i) {
  return {
    name: 'Object' + i,
    print: function () {
      return this.name;
    }
  };
}
```

And this as a prototypal alternative:

```
function MakeObj(i) {
  this.name = 'Object' + i;
}

MakeObj.prototype = {
  print: function () {
    return this.name;
  }
};
```

I created array of 1 million objects and measured the results:

- Chrome: 356MB for literal, 11MB for prototypal
  - literal syntax used roughly 32x more memory
- FF: 114MB for literal, 8MB for prototypal

The verdict is grim for the object literal approach.

## Conclusion

This wasn't the most rigorous scientific test, but I think the results are
still convincing-- and frankly disappointing.

I think that JavaScript's `this` keyword and prototypal inheritance are 
misleading features (at least to most polyglot developers). So, I was hoping to 
see simpler alternatives win out.

Modern browsers are highly optimized for certain patterns, and prototypal 
inheritance is king here. Simulating OO using object literal syntax is simply
not a scenario that has been optimized. So, each time an object literal is
created, a copy of each of its methods is created. 

It is important to note that object literals, when used simply as data records,
perform just fine. (See the `None` test in the JS Perf examples.) It's when
they're used to attempt OOP that they fail to perform.

Premature optimization is evil. But, when you can follow a simple rule of thumb
such as "prefer prototypal objects" and get free perf gains from it, the choice 
is obvious. As for me, I might start following this rule of thumb: "screw OO, 
let's just do FP".
