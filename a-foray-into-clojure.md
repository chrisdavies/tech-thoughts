# A foray into Clojure

I noticed a bug in our product where images sometimes get stored as data urls in text blobs in our database. I wondered how many people were affected by this and how much space was taken up by this extra data. But it's pretty boring to compute that in Ruby or C# or whatever. So I decided to try it with Clojure. It's my first time ever writing Clojure. Here's how it went.


## Getting set up

Getting set up (on my Mac) was stupid easy. 

`brew install leiningen`

Then, I installed a [decent editor](https://sekao.net/nightcode/) to help manage all of the parenthesis.


## Stay strong and repl on

Running `lein repl` kicks up a nifty little command line tool for evaluating Clojure. But in the end, I prefered using the Repl built right into Nightcode (the aforementioned editor). I blew it up a few times. It's a handy feature, but if you're dealing with large volumes of data, you may want to tread lightly.

Basically, my Clojure development process was:

- Break the problem down into a little functional recipe
- Use the repl to figure out how to code a function
- Code the function
- Use the repl to test the function
- Move on to the next function in the recipe... rinse, repeat


## Managing project.clj

With any project of substance (even this little one), you need to be able to pull in dependencies. Unfortunately, Leiningen doesn't appear to have an equivalen to `npm install --save <packagename>`. So finding the correct (latest) version for each package is kind of painful.

I had to Google a good bit. Some were located on one site, others on GitHub, others on random blogs. I feel like I *must* be missing something, because I can't imagine the Clojure package finding process being so manual.

Anyway, my project.clj ended up looking like this:

```clojure
(defproject stats "0.0.1-SNAPSHOT"
  :description "Whatevz"
  :dependencies [[org.clojure/clojure "1.8.0"]
                 [org.clojure/java.jdbc "0.6.2-alpha3"]
                 [mysql/mysql-connector-java "6.0.3"]]
  :javac-options ["-target" "1.6" "-source" "1.6" "-Xlint:-options"]
  :aot [stats.core]
  :main stats.core)
```


## Nulls, substr, and so forth

I haven't done much functional programming, but Haskell and F# have both made me forget that null exists. So, my Clojure exploded a few times with null pointer errors. Also, Clojure's substring function explodes if you get your bounds wrong, so I had to write trunc.

```clojure
;; Make sure we have a non-nil string
(defn safe-str [s] (if (nil? s) "" s))

;; Truncate a string safely
(defn trunc
  [str n]
  (let [s (safe-str str)]
    (subs s 0 (min (count s) n))))
```


## Out of memory when processing big database tables

Turns out org.clojure/java.jdbc's query isn't lazy by default. I guess it makes sense, as it'd have to hold the database connection open for an undefined amount of time.

The table I was processing is roughly half a gig, which is actually pretty small. It can easily fit into my Mac's memory. Still, slurping it into an array caused a nice out of memory exception.

Well, there's a pretty handy way to lazily process records: `result-set-fn` as somewhat documented [here](http://clojure-doc.org/articles/ecosystem/java_jdbc/home.html).

```clojure
;; Load up activities and crunch numbers and such
;; This passes a lazy sequence of hashes/maps into the compute-stats 
;; function. Whatever compute-stats returns is also returned by j/query.
(defn get-activity-stats []
  (j/query db
    ["select description from activities"] 
    {:result-set-fn compute-stats}))
```

The `compute-stats` function is one that I defined elsewhere. It can just treat its argument like any ol list of maps. Nice.


## No static typing

Well, Clojure has some extension you can use to slap static typing onto it. It seems intrusive, from what I've seen. I wish that you could just optionally tell the compiler to do F#-style type-inference and analysis.

A number of times, I called a function with the wrong number (or order) of args, and Clojure compiled just fine, only to crash and burn when I ran the app. This made me sad.

When you've tinkered with F# or Elm, you tend to forget that some functional languages have runtime errors. (They happen in F# and Elm, too, but much less often).


## Parenthesis

The parenthesis do make the code hard to read in some cases: "Which one of these 7 parenthesis is the end of the if statement?" But I think it's just because I'm not used to them and because I haven't learned to trust Nightcode. 

Nightcode colors each set of parens, making it easier to match them. It also does some crazy smart auto-closing of your parens. It's awesome, actually. I think that all of my problems with parens are really non-issues when you just type what you want and let Nightcode magic your parenthesis into place.


## Cons

If I had to come up with a list of cons right now, they'd be:

- Nulls
- Leiningen doesn't seem to have an `install --save` option
- Startup time is kinda slow (I'm going to blame the JVM)
- Static typing is a 2nd class citizen
- No automatic currying
- JVM

I tacked that last one on there, just because I can. It's obviously a pro, too, depending on how you look at it. 


## Conclusion

Would I recommend Clojure? Yeah. I would.

It's a Lispy language that is well designed and actually has traction (sorry PicoLisp and Chiken Scheme).

It's enjoyable to work with. I was up and productive with it right away, the repl is handy, and the errors are clear (even the runtime ones).

How does it stack up against other languages I've tried? Hard to say.

Based on this brief excursion, I prefer it over Ruby. But then again, I prefer *any* functional language to Ruby. However, most functional languages that I like are hard to recommend to real businesses. Their tooling and ecosystems simply aren't as good as that of mainstream languages. Clojure stands out as something of an exception in this regard. It's a functional language that I like, and it's got solid tools and a solid ecosystem.

For big projects, I'd still lean towards a statically typed language. I've been bitten too many times in our Rails/JavaScript codebase by silly problems that never existed when I wrote C#. Clojure's dynamic nature leaves it open to exactly the same set of problems.

If F# had the vibrant and robust community that Clojure does, it would be my language of choice, hands down. But it doesn't. So it isn't. Moving forward, I'm going to do more of my tinkering with Clojure. It just may become my language of choice.
