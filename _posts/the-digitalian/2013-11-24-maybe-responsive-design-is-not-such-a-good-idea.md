---
layout: post
categories: 
- digitalian
- commentary
comments: true
title: Maybe Responsive Design is not such a good idea?
image:
  feature: birch-trees2.jpg
  credit: Francesco Gallarotti
  creditlink: http://www.gallarotti.net
---
![](/assets/2013/11/responsive.jpg)

While working on a side project and researching about *node.js*, I stumbled upon [an interesting comment](http://venturebeat.com/2012/05/02/linkedin-ipad-app-engineering/), made by Kiran Prasad, head of the LinkedIn's mobile development team.

>Responsive design might work for uncomplicated, one-off websites, but for applications or networks (such as LinkedIn is), responsive design is actually bad. We’re looking at

> - the entrenched use case [for desktop users],
> - the coffee-and-couch use case [for tablet users],
> - and the two-minute use case [for mobile phone users].

> You can’t take a mobile app and just scale it up to tablet or desktop. A lot of responsive design is building one site that works everywhere. This might work for some websites. But it’s a bad approach for others and especially for apps… You have to come up with a completely different design for each of the above use cases.

It made me think, and brought back to mind what [a colleague](http://peteiuvara.com/) recently recommended to a client (a cabinet maker company) when they asked about building a mobile version of their website. Others might have jumped on the opportunity and would have suggested a complete website redesign to make it responsive. Instead, he brought up the use case of the user who access their site from their mobile phone. "*When could that happen?*" he asked. "*When would a person browse a cabinet maker's website on their mobile phone? It could happen when they are at the Home Depot, looking for cabinets and they want to make some quick comparisons. So no need to put on the mobile website any media heavy content, no need to focus on the company history, or on the cabinet maker application. Just simple info about their products, with measurements and prices.*"

**I like his approach, use-case based**. As software engineers we often end up falling to deep into the technical trenches and **forget about the end user**, who in fact are the people for whom we are ultimately building our products. And working from a use case perspective, responsive design, makes really no sense, because, even though it's a really cool concept from a technical perspective, it doesn't necessarily address any of the needs of the end users. It seems to me that it's just another case of "*just because you can do it, it doesn't mean you should be doing it*" type of scenario.