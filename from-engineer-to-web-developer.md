# From Engineer to Web Developer

I've been programming professionally for 16 years now. In that time, I've architected several systems, and been involved in many more. I've done some interesting work, and lots of mundane work. I've enjoyed my career and hated it.

In the early days, my dev process looked like this:

```
Gather requirements ->
  Reflect on the problem ->
  Prototype ->
  Gather more requirements ->
  Prototype, etc ->
  Architect a solution ->
  Impliment the solution ->
  Iterate
```

It was a fun process. I often got to interface directly with customers. I got to design the UX, build the servers, and everything in between. I've written distributed, in-memory data services for Microsoft, a system to manage a distributed network of proprietary hardware for a major shipping terminal, multi-threaded proxy servers to put a more modern face on old COBOL applications, scriptable form designers, etc.

But in the last, oh, 5 years or so, I've become a "web developer". It started with a job at a consulting firm on the east coast, and progressed slowly from there.

Over time, my dev process turned into this:

```
Gather (some) requirements ->
  Shoehorn the solution into the Model View Controller pattern ->
  Call it a day
```

99.9% of all server-side code is nothing more than this:

```
Request ->
  Authenticate ->
  Authorize ->
  Validate ->
  Persist (to one or more stores) ->
  Build Response ->
  Notify of Result ->
    (Render .erb, JSON, send emails, SMS, websockets, etc)
```

As for the client-side, it's a tangle of state, event handlers, and questionable separations of concerns, but that's a subject for another post.

On a day-to-day basis, I no longer ask myself, "What is the root problem we're trying to solve? What is the best way to solve it?"

Why don't I ask these questions?

First, the question, "What is the root problem?" is the responsibililty of the business developers, not mine. In past jobs, it *was* my responsibility (or at least the dev team's responsibility). Finding the problem, designing the solution, and delivering it was almost entirely within our control. (To be fair, I still make this my problem, but the barrier to properly answering this question has become much greater due to separation of roles and the distancing of engineers from real-world clients.)

Second, the question, "What is the best way to solve it?" has already been answered. MVC, whether Rails, or ASP.NET MVC, or Django, or whatever. If there's an existing system in place, you'd better have a damn good reason to *not* use it. If there's not an existing system in place, you'd better have a damn good reason to *not* use an out-of-the-box solution.

In many ways, this shift is a *good* thing. You don't need a degree in theoretical computer science to solve most of today's problems. Tries? Quaternions? AVL trees? Nah. Don't need to know about em. Everything's been nicely solved and abstracted behind a library/framework. Just `gem install xyz` and move on.

Programming is more accessible today than ever before. But the work is also more mindless. It consists mostly of cobbling together a bunch of loosely associated libraries and tools, making liberal use of duct tape, then grabbing a beer.

And the strangest part of all is that this works pretty well for the majority of cases. The solutions aren't ideal. The code is bloated and slow, but we just throw more servers at it, and it trundles along. It's got lots of duplication and warts... But mostly, it works.

I maintain my interest in the industry by learning interesting languages (currently OCaml and Haskell), feeding my [Hacker News](https://news.ycombinator.com) addiction, and occasionally grabbing beers with fellow nerds. Solving interesting problems? Not so much. But now that I've stopped to reflect on it, this seems rather sad.

Is it all doom and gloom? No. I work with some great people. I get decent pay and great benefits. I write software that people use *and love*. And-- maybe best of all-- I work remotely (it's hard to overstate how much I love this).

As for my monody about the state of the industry? Constructive reflections are forthcoming...
