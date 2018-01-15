---
layout: post
title: "Delayed Job as a rake task"
subtitle: Running delayed job on-demand with a rake task and Heroku Scheduler
author: Awin Abi
date: 2018-01-15 17:32:00 +0530
categories: technology rails
imageurl: perfrails.jpg
---

Delayed Job is a great and simple edition to background jobs in a Rails application. But if you don't have the memory or resources to run a background thread 24hrs on a server what do you do?

<!--more-->
We were in a similar situation for a client, who was strictly on a shoe string budget, running off Heroku.
The background job was to process some applications, verify and sent emails if they had errors. This was to be done once a day. Running a background job just for this was a bad choice.


Another option was to run this as a rake task, at a set time, use [Heroku Scheduler](https://elements.heroku.com/addons/scheduler) to run this at 04:00am every day. But we wanted to try a bit more generic solution.


We decided to stick to write Delayed Job classes, so that in future, it can serve 2 purposes

1. For more background processing, more jobs we just have to write new classes with `perform` methods
2. When the app is moved into it's own private hosting VPS, run a background thread more often, to scale it.

So delayed job it was!

But in the current setup, we had to run the jobs as a rake task, triggered once a day at a specific time, while leaving room for a full fledged background process in the future.

Looking at the [delayed_job source at Github](https://github.com/collectiveidea/delayed_job) we figured that this could be easily implemented. It internally creates a `Delayed::Worker` class and works off all the jobs in the `delayed_jobs` table.

The solution was really simple.
Create a rake task in `lib/tasks/delayed_job.rake`

```ruby
# File lib/tasks/delayed_job.rake

namespace :delayed_job do
  desc 'Run Delayed job worker'
  task work: :environment do
    worker = Delayed::Worker.new
    worker.work_off #by default runs 100 rows
  end
end

```

**For more information on the sources:**

- The rake task for starting delayed job - [delayed/tasks.rb](https://github.com/collectiveidea/delayed_job/blob/c7a42fb78dc538de46024eb36b31e72916ff21a0/lib/delayed/tasks.rb#L8)

- The Delayed::Worker start - [delayed/worker.rb](https://github.com/collectiveidea/delayed_job/blob/c7a42fb78dc538de46024eb36b31e72916ff21a0/lib/delayed/worker.rb#L156)

- Delayed::Worker work_off definition - [delayed/worker.rb#work_off](https://github.com/collectiveidea/delayed_job/blob/c7a42fb78dc538de46024eb36b31e72916ff21a0/lib/delayed/worker.rb#L206)
