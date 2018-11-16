# Killing the Productivity Killers

## The $26,000 Part

Late in my dad's career, he was asked to turn around a manufacturing facility that was bleeding money at the rate of roughly $150,000,000 per year. While touring the facility, he noticed a critical machine sitting idle.

"Why isn't that running?" he asked.

"It's missing a part that costs $26,000, and no bank will loan us the money," replied the tour guide.

Dad ran some back of the envelope calculations and realized that when running properly, the machine would make roughly $100,000 in new prodcuts *per day*. So, a one-time purchase of a $26,000 part would turn into an endless stream of money.

Surely, the employees of that company could have put their heads together and scrounged up $26,000, but no one ever took the initiative. How does this happen?

Reflecting on my career as a software engineer, I have seen similar problems play out over and over again.

## The Credit Union Company

My very first software engineering job was for a credit union software company. The day before I walked through its doors, its very last programmer had quit. The company was desperate, so they hired me and my friend Levi-- two college juniors-- and asked us to start right away.

Our first assignment was to fix bugs in their primary loan product. We sat down and got to work. It was a ratsnest of C++, written by a COBOL programmer who hadn't bothered to learn C++. It had been in development for 5 years and had never shipped. Half of the time, it wouldn't even launch. When it did, it would crash and terminate randomly after running for only a few minutes.

We decide to scrap it and rewrite in in .NET. We never bothered asking permission, and no one was the wiser until we had it working. They were used to having months of zero-productivity on this product, so we knew we could get away with it. It took us around a month to get a working product up and running, and maybe another month before it was fully-featured enough to ship to the first customer. 

The rewrite was such a success that we were then given free-reign to do whatever we wanted. We rewrote their middle-tier services, and teller product. Then, we wrote an internet banking product, a phone-banking product, a credit reporting solution, and quite a bit more. Within two years of us walking through their doors, this company had a full-suite of products that they never dreamed possible.

## Big Printing Co

In another job, I worked for a big printing company whose brand name you'd recognize. They hired me to come in and build a scripting language to help them speed the time-to-market for their installers. Compiler design was probably my favorite course in university, so I jumped in with both feet. But it was a bait-and-switch. The first day on the job, it was clear that I wouldn't be doing anything interesting for a *long* time. "We just need to get more manpower on writing these installers right now," I was told.

They had a team of maybe twenty people working full time on writing installers-- a job which, given the proper tooling-- could probably be done by a team of two or three. But they weren't willing to put resources towards building the tooling. Instead, they had an ever expanding team of programmers doing mundane work with subpar tools.

## Big Software Co

This project again involves a household name. I was put on a team of three developers whose job was to create cloud deployments for some of their major services. The job mostly involved verifying builds, editing some *massive* XML files, and a bunch of similar tedium-- exactly the sort of thing that computers are good at and people are not. I asked if I could just build a tool to do this, but was told "No. We don't have the budget or resources for that."

I did it anyway. It took about a week to get a working proof of concept.

The job went from 120 tedious and error-prone man hours per week to 3 relatively straighforward ones.

Suddenly, my team had whole lot of time on their hands, and we all moved on to more productive things.

## Wee Little Software Co

Another product I worked on was a web platform for teaching courses. Its highest tiered package provided hand-crafted custom styles for our clients. These customizations were stored as template and SCSS overrides in the codebase itself. There were hundreds and hundreds, maybe thousands of such files. It was not systematic. You couldn't make a change to any part of the site without worrying that you might have broken one of the myriad customizations. It slowed development of certain features to a crawl.

One of our developers spent several months modernizing the site-- a task which should have taken a few weeks, but these infernal customizations bogged everything down.

This was a clear case where "Make the change easy, then make the easy change" would have been a great strategy.

I recently advised this company to invest time in systematizing the customization layer. Don't store customer customizations in the codebase-- drive them from the database via data that can be reasoned about, systematically upgraded, and systematically tested. If the customization system works after you make a change to the site, you can be confident you didn't break any of your customers.

## Lessons

How does this happen? How do companies filled with smart people fall into such obvious and avoidable traps?

There's a real difference between intelligence and leadership. Most of these places were filled with intelligent people, but those people were unwilling to take risks, incapable of stepping up and leading. Leadership is a *really* tough quality to find.

Inertia tends to force us down the easy path. It's not easy to change direction. Change feels risky. Investment-- almost by its definition-- is risky. In each story, we saw how a little upfront thought and investment in proper tooling would yield massive rewards. Buying the $26K part to fix the machine was a no-brainer. Automating the deployment process at Big Software Co was a no brainer. Investing in proper tooling at Big Printer Co was a no brainer. Often, these investments yield immediate rewards. Yet, convincing stakeholders to make such investment has consistently proved difficult. I think the reason for such resistence to change is the perceived risk. I've personally come to the conclusion that the easiest way to convince stakeholders is to just make the investment yourself. Ask forgiveness, not permission. When they see the actual solution working and making them money, they will listen to what you have to say.

Companies tend to focus on immediate, small wins, at the cost of missing much larger medium-to-long-term opportunities. In some cases, you can see the [sunk-cost falacy](https://en.wikipedia.org/wiki/Sunk_cost) at work. "Well, we already spent 5 years on this C++ quagmire. We can't just scrap it and rewrite it!" In other cases, we see the "If it ain't broke don't fix it." mindset. "Sure this process of building installers is labor-intensive, but it ain't broke..." Good leaders help shift these myopic perspectives, help others to see what is possible. You don't need to be in management to be such a leader.

It has often been observed that junior programmers are overeager to rewrite a thing. This is true. But the reverse is also true. Senior programmers and managers are often *far* too conservative. Sometimes rewrites are the fastest way forward. I've written more on this [here](https://github.com/chrisdavies/tech-thoughts/tree/master).

Rooting out and correcting these sorts of inefficiencies is part of what it takes to become a 10-xer. Identifying these problems is not always easy. For example, at Wee Little Software Co, it took me a full year before I realized that one of the biggest productivity killers was our customization mechanism. The key to such insights is to step back, and try to gain a little perspective. Find a good hammock, and do some deep thinking and reflection. Ask, "What are people spending their time on? Why?" ["Why?" is probably the most important question you can ask.](https://en.wikipedia.org/wiki/5_Whys)
