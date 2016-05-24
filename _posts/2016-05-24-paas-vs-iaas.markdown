---
layout: post
title:  "Why PaaS(Heroku) when there is IaaS(AWS)?"
subtitle: Compare PaaS with IaaS
author: Charles Skariah
date:   2016-05-24 20:08:37 +0530
categories: technology cloud
imageurl: heroku-vs-aws.jpg
---

Reasons are many, to start with mainly PaaS(Platform as a Service) gives you an environment where you just push code and some basic configuration and get a running application, where in IaaS(Infrastructure as  a Service) it gives you components you need in order to build things on top of it.

To build a production server in AWS, a bulk of configurations needs to be done starting from web servers like Passenger/Unicorn and a Nginx front-end to handle the requests. Also you need to put additional effort for setting up a database like MySql or PostgreSQL. Finally you need to get help from deployment tools like Capistrano, Mina etc to handle the deployment.

It’s really consumes a good amount of time for a newbie, where as in heroku you can make it with few lines of code and a git push - and everything works like magic!

<h2>Speed</h2>

Scaling is pretty much easy with Heroku. By using the web interface in Heroku, you can easily scale your dyno by just scrolling to the required numbers! Since we’ll all have scaling problem at some point of time, using Heroku’s architecture, one can easily find a solution for this without spending much time on it. The learning curve is almost close to zero, and anyone can get used to this in a matter of minutes.

Coming to AWS, we need to have a set of configurations particularly for your application, to manage the workers and dynos matching.

<h2>Scaling</h2>

Scaling is pretty much easy with Heroku. By using the web interface in Heroku, you can easily scale your dyno by just scrolling to the required numbers! Since we’ll all have scaling problem at some point of time, using Heroku’s architecture, one can easily find a solution for this without spending much time on it. The learning curve is almost close to zero, and anyone can get used to this in a matter of minutes.


Coming to AWS, we need to have a set of configurations particularly for your application, to manage the workers and dynos matching.


<h2>Cost Efficiency</h2>

Regarding cost-efficiency, different people have different opinions. Right now Heroku is running $0.05/hr for a dyno, where as the price is almost half for an AWS micro instance.

Also Heroku gives 512 MB of RAM in a dyno, so we can’t really compare the same with ec2 micro instance.The only question is, does it have the price of almost double? Also considering the time saved, I think it’s not ‘huge’ for small scale applications.

<h2>Conclusion</h2>

What Heroku offers for web developers is instant deployment, fast & easy scaling, and vast tool selection. Now we can concentrate on building our applications and forget the tedious deployment and server administration tasks that used to strangle our productivity. We can deploy fast, scale quickly, and adjust to circumstance as needs arise. The game is changing, and Heroku (and others like them) are changing it.


We love Heroku for "quick deployments". When we start an application, and we want some cheap hosting (the Heroku free tier is awesome - essentially if you only need one web dyno and 5MB of PostgreSQL, it's free to host an application), Heroku is the  go-to place. For "serious” production deployments with several paying customers, and a service-level-agreement, with dedicated time to spend on deployment etc, we can't quite bring ourself to offload much control to Heroku, and then either AWS or our own servers have been the hosting platform of choice.


Ultimately, it's about what works best for you. For "a beginner programmer" - it might just be that using Heroku will let you focus on writing Ruby, and not have to spend time getting all the other infrastructure around your code built up.


Also right now developers deploy their code written in Node, Ruby, Java, PHP, Python, Go, Scala, or Clojure to a build system which produces an app that's ready for execution.



*Disclaimer*: We just discussed only pros of heroku, there are a lot of cons including show stoppers for various applications, which we can discuss on future posts.
