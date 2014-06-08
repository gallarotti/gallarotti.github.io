---
layout: post
categories: 
- digitalian
- commentary
comments: true
title: How to Minimize Distractions Recovery Time 
image:
  feature: clouds.jpg
  credit: Francesco Gallarotti
  creditlink: http://www.gallarotti.net
---
The [Norwegian Developers Conference 2014](http://www.ndcoslo.com/) just ended and they have already posted [all the videos](http://vimeo.com/ndcoslo) of all the talks that were given this past week. Two talks in particular caught my attention. 

The first is a talk about [Railway Oriented Programming](http://vimeo.com/97344498) (an interesting manage error handling in functional languages) by [Scott Wlaschin](https://twitter.com/ScottWlaschin). Because I don't actually use functional languages I won't comment any further about this talk and I will limit myself to bringing it to your attention, just in case you also might be intrigued by the way Scott uses the railway methaphore to illustrate a concept that might be otherwise difficult to explain.

The second talk is [Zone out, check in, move on](http://vimeo.com/97419151) by [Mark Seemann](https://twitter.com/ploeh). I hold Mark in high regard -- he is a brilliant .NET architect, a prolific [blog](http://blog.ploeh.dk/) writer, and the author of [Dependency Injection in .NET](http://www.manning.com/seemann/), Amazon #1 best seller in the Software Testing category. If you have one hour of time to spare, I highly recommend [watching the entire talk](http://vimeo.com/97419151). If not, I asked Mark permission to summarize here some key ideas that particularly resonated with me.

Most programmers love to be 'in the zone' as much as possible. This is what makes developing an enjoyable experience. However, no matter if you work in an office or if, like me, you have the luxury of being able to work from home, the reality is often one of interruptions. Interruptions are not only unavoidable, but they tend to increase in number when a developer grows into a senior developer first and a lead developer next.

[Bruno Oliveira](https://plus.google.com/+BrunoOliveira/posts) once posted a fun diagram which depicts the productivity at work of a developer along the day:

![Geek Productivity at Work - by Bruno Oliveira](/assets/2014/06/geek-productivity.png) 

Humor aside, the smaller diagram in the top-right, which shows the effect of a 5-minute interruption (or a meeting) on productivity, turns out to be surprisingly accurate. First of all, we need to better understand why it takes so much (15-20 minutes) to go back to the same level of productivity. After all in a lot of different professions this does not seem to happen. When I am at the veterinarian's office for my dog's check-up, for example, he gets interrupted at least half-a-dozen times during the 30 minutes of my appointment, yet he seems to handle those interruptions really well and his recovery time is just a few seconds. 

The reality is that, quite ironically, developers tend to be very bad at multitasking because when we write code we need to constantly keep in our heads a mental model of the problem at hand (i.e. the classes involved, the relationships between them, the 'stacktrace' of the code execution, corner cases, etc.). In essence, while developing code our brains simulate the work of an interpreter: we read source code, we build a model of what we think that code will be doing when executed, and we execute this model in our head, line by line. Whenever a distraction comes along, during the post-distraction recovery phase we find ourselves retracing our steps by reading again bits and pieces of source code and rebuilding that model in our head. This takes time and is also a tiring process - which means that every recovery phase will take a bit longer than the previous one.

Next, Mark goes over a few different strategies to help us cope with these interruptions. Since interruptions are unavoidable, he focuses on a few different strategies that can help us shorten the recovery phase, since after all, it is only after that phase is completed that we are able to get back 'in the zone', the only place where developing becomes enjoyable again. Of these strategies I'd like to summarize two which particularly resonated with me.  

### 1. Use a smaller screen

This might seem counter-intuitive especially since as programmers we have spent the last few years asking our bosses to purchase more and bigger screens for us, claiming that more screens would allow us to be better productive. In reality though more screens inevitably provide us with more distractions (incoming emails, Lync, Skype, too many documents open, etc.). 
Mark says that a smaller screen (in the video Mark shows a screenshot of his 13" screen laptop during a coding session) motivates him to decompose the code that he writes in reasonably sized parts that allow him to keep everything under control by just looking at the screen.   

>Any fool can write code that a computer can understand. 
>Good programmers write code that humans can understand. *Martin Fowler, 2008*

Write good code requires creative thinking, and, as with many other creative endeavors, often introducing artificial constraints in the creative process sparks creativity. "It makes you think out of the box because you are being put into the box" says Mark.

The size of the screen though really doesn't matter. What matters is the fact that by limiting the size of the screen he found himself more and more applying the "divide and conqueer" strategy to his work. Instead of working on very large scale systems in their entirety, he started decomposing them into smaller loosely coupled units which obviously, by their intrinsic nature of being smaller, both in size and in complexity, help shortening the distraction recovery time -- one more valid reason that adds itself to all the other advantages of decomposing large solutions into smaller parts.

### 2. Use a Distributed Version Control System (DVCS)

We tend to think of a version control system as a way of saving, once we are done, all the code that was developed for a particular task / feature, into a safe location. The name of Visual Source Safe highlighted this aspect of what a VCS is for.

Using a DVCS enables you to **switch context painlessly**. When you want to see the state of your code in a particular branch (i.e. `feature-42`), in `git` it takes just one line: 

    git checkout feature-42

and almost instantly, because everything happens locally on your machine with no network connectivity needed, the working directory will be updated with the files from branch `feature-42`. As soon as you want to switch back to the `master` branch, all you have to do is type:

	git checkout master

When you want to create a new branch (i.e. `feature-43`), another line is sufficient:

	git checkout -b feature-43

Remember, this is a local branch, no network is involved, everything happens just on your local machine, which make the branch creation a sub-second operation.

**Working in branches** is not an evil thing. The people that are scared of branches think of **merge conflicts** (which can admittedly be hell). But merge conflicts are not triggered by the mere acting of branching. 

Instead, **working in branches** enables you to **explore fearlessly**. You have an idea, you simply create a new branch, and you start committing to it, as often as every few minutes (Mark actually says "never more than 5 minutes from a check-in" -- see below):

![branching](/assets/2014/06/branches-01.png) 

If I figure that, for example, the last few commits I made were a mistake, but the initial idea of this branch was good, I can rollback the last commits, and then try something different:

![branching](/assets/2014/06/branches-02.png) 

When you think that what you have is good, you merge back to the master branch:

![branching](/assets/2014/06/branches-03.png) 

    git checkout master
    git merge feature-43 

All of this, by the distributed nature of `git`, happens on your local machine. No one else sees it. Once you merge to the master branch, you can then push to the shared repository so that everyone else in the team can see your code. This keeps your source control history clean and keeps the number of commits down.

#### Never be more than five minutes from a check-in 

Every time you make a change to your code that leaves the system in a state that is as good as it was before, or better (i.e. not worse), just check the change in. The more commits you have, the larger number of options you have when rolling back if necessary (every check-in gives you an "undo snapshot" of the system, to which you can easily go back).

#### Integrate often

> Branching is not evil, but merging is.

**Merge conflicts** are nothing else than **concurrency issue**. It means two or more developers have been working on the same piece of source code in between two merges. If there were no concurrent work being done on the same file, there would not be a merge conflict. In other systems to resolve concurrency issues the normal approach is to make sure that transactions happen as fast as possible. 

Therefore this translates into doing merges as often as possible. This minimizes the chance of having a merge conflict when merging because it reduces the window of time during which a piece of source code risks to be modified by more than one developer. A branch that gets older than a couple of hours should be merged. 

#### Handle incomplete branches

Often it happens that a feature (i.e. `feature-42`) is not completed within a few hours and you understand that it will take much longer to be completed:

![branching](/assets/2014/06/branches-04.png) 

Most people would leave the branch there dangling waiting for the branch to be completed, but, in most cases, the best way to handle this scenario is to simply integrate the feature anyways into the master branch:

![branching](/assets/2014/06/branches-05.png) 

This is because most often:

> The cost of having merge conflicts is bigger than the cost of having incomplete features in the master branch. 

And if you don't like to have incompleted features in the master branch, all you have to do is to use 

> We don't know how to minimize the cost of merge conflicts, but we know how to minimize the cost of having incomplete features in the master branch, by using **feature toggles**.

**Feature toggles** can be configuration files, or complex boolean expressions in code that determine whether a feature should be turned on or off in a particular environment. In order to be able to use feature toggles, your code will need to be modular enough to be "wrappable" by the toggles, but then again this brings back the point about the advantages of writing loosely coupled code.






