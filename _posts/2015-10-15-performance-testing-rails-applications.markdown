---
layout: post
title: "Performance Testing Rails Applications"
subtitle: Tools to measure the performance of a rails application
author: Awin Abi
date: 2015-10-15 18:32:00 +0530
categories: technology rails
imageurl: perfrails.jpg
---

Measuring the performance of a rails application is very important before it is deployed to production. Rails framework provides various tools to measure the performance of the application.

Performance tests are typically integration tests that helps identify the application's memory and speed issues.

<!--more-->
<h2>Setting Up</h2>

You can use the rails-perftest gem to benchmark and profile rails code.
Include the following gems in your project's Gemfile

```
gem 'rails-perftest'
gem 'ruby-prof' # for MRI profiling

```


<h2>Writing performance test cases</h2>

As mentioned before performance tests are special integration tests in the rails application.
Assume that you would want to test the dashboard of your application which has the following code:

```ruby

# app/controllers/dashboard_controller.rb
# accessible via route /dashboard
class DashboardController < ApplicationController
  def show
    @comment_list = Comment.where("commentable_id IN (?) AND commentable_type = ?  AND commented_by != ? AND description != ? and visible = true", completed_evaluations_id ,'EvaluationInstance',current_employee.id,"").order('created_at desc').limit(10)
  end
end

```
This dashboard action gets the latest feedback of given to the user in the form of comments. The performance test for this action can be something like the following:


```ruby

require 'test_helper'
require 'rails/performance_test_help'

class PostPerformanceTest < ActionDispatch::PerformanceTest
  def setup
    login_as(:employee)
  end

  test "Access Dashboard" do
    gte '/dashboard'
  end
end

```
The performance test will provide details like the time take for the action, the number of objects created and total memory used for this.

Similary a model performance test can be used to give an idea on the time required for a function. This can come in handy while running complex queries against the database.

```ruby

  test "slow method" do
    comment = FactoryGirl.create(:comment)
    comment.slow_method
  end

```

<h2>Running performance tests</h2>

Performance tests can be run in two modes - benchmarking and profiling modes.

Benchmarking makes it easy to quickly gather a few metrics about each test run. Each test case is run multiple times( by default 4 times) in benchmarking mode.

Performance test can be run in benchmarking mode using

```sh
$ rake test:benchmark
```

Profiling does an in-depth analysis using an external profiler. For MRI this is done using RubyProf. Here each test case is run only once.

Profiling can be done using:

```sh

$ rake test:profile

```

Benchmarking and profiling gives multiple metrics as results including various time, memory and garbage collection metrics. More details can be found [in this readme page](https://github.com/rails/rails-perftest#metrics)

<h2>Command line tools</h2>


For quick and dirty performance testing(without writing performance test files), this also provides command line tools `benchmarker` and `profiler`. Each of this command can be run by passing a snippet of code to it.

An example run could be as follows:

```sh
$ perftest benchmarker 'Comment.all' --runs 3 --metrics wall_time,memory
```



Performance tests are a useful tools to measure, benchmark and profile blocks of code in your rails applicaton. A well written performance test can help a developer to analyse the most time taking parts of the key execution paths in the application.

