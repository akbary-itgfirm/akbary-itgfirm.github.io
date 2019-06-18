---
layout: post
title:  "Testing Orchestration: a Serverless Microservice Case Study Part One"
date:   2019-03-26 11:09:00 -0400
categories: ["serverless"]
tags: ["serverless", "cucumber", "selenium", "testing"]
---

Chances are, if you’re developing software right now, you are not developing serverless software. It’s a fun and exciting technological paradigm shift in computing, but is not exactly industry standard or compatible with the majority of existing applications or platforms most commonly used today.

Chances are, you’re building your custom blog with WordPress, Continuously integrating your software with Jenkins and using Oracle DB for your database needs. Although there are serverless alternatives to these, most prefer the convenience of being able to use established, industry proven, and highly supported technologies to get off the ground as quickly as possible.

How, then, can one leverage the advent of serverless computing to simplify life and optimize costs for these projects? The answer I have found in my development as a DevOps Engineer at ITG, has been to utilize these technologies to provide additional capability that is entirely decoupled from the application it is meant to support.

## Case Study

As an example, consider a scenario involving the need to perform automated regression testing on a web application hosted on an EC2 instance. In this scenario, testers are black box testing the application (testing without knowledge of its implementation or need to coordinate with the impelmenters) and executing testing scripts through utilization of [Cucumber][cucumber-homepage], [Selenium][selenium-homepage].

![Cucumber][cucumber-photo]

A perfectly acceptable implementation of this solution would be to have testers execute their cucumber scripts from their local devices, allowing a selenium webdriver to hijack their computer, executing test step by test step until completion of the feature file.

# Advantages

The advantages of such an implementation is the relatively low LoE and configuration management required. You could probably get off the ground in an hour or two following the links provided above. Futhermore, modifications are relatively easy to implement. Changes to step definitions made locally provide really quick updates to functionality.

# Disadvantages

The disadvantages of this implementation almost deifnitely outway the negative for any meaningful sized project, however. For one, the execution speed of such an implementation is likely to be extremely slow. Driven by the local machines of testers, execution speed is going to be bound by:

<p align="center">
( Cucumber step execution duration ) X ( # of steps ) X ( # of features )
</p>

Testing is going to be slowed down even further by failed test cases. Upon failure, adjustments are going to be made, or development remediation is going to be addressed only to repeat the testing process.

<p align="center">

  <img alt="This took way too long." src='https://g.gravizo.com/svg?
    digraph G {
      Start [color="green", style="filled"];
      End [color="green", style="filled"];
      Test [color="yellow", style="filled"]
      Remediation [color="red", style="filled", weight=8];
      TestFailure [color="red", style="filled"];
      Start -> Test;
      Test -> Remediation [weight=8];
      Test -> TestFailure [weight=8];
      Remediation -> Test;
      TestFailure -> Test;
      Test -> End [weight=12];
    }'
  />
  
</p>

To some extent, these delays are entirely unavoidable. Remediation of defects introduced during development is going to impede progress of test completion, and errors in testing scripts are going to do the same. That does not imply that there isn't a better way to be executing these scripts, and that time can't be saved. Features can be executed in bulk on a server to speed up testing, and a [Selenium Grid][selenium-grid] can be used rather than a standalone Selenium WebDriver to allow for simultaneous execution of test cases.

Futhermore, management of test case libraries and step definitions can be complicated when executing off of local machines. Generally speaking, it's preferable that a shared baseline is used when validating software functionality. It would be unfortunate for a piece of software to pass testing only to be invalidated by an update to the testing suite. Coordination of the different components here can also be difficult. Different code bases from repos that are out of sync can invalidate test executions, and rolling out coordinated updates to Cucumber, Selenium and the Selenium WebDriver to local machines can be complicated.

Additionally, visibility of test cases is hindered if one has to dive through different branches of a git repo. Ideally, step definitions are isolated from test cases so that the advantages of [Behavior Driven Development][behavior-driven-development] can be leveraged. Cucumber scripts are fairly easy to read and require little to no technical expertise to validate. Technical implementations of the steps involved with them can be fairly nuanced, however.

# Decoupling Services

In the next post on this subject, the topic of service decoupling will be explored to attempt to address some of the major concerns brought up here. We will look at the use of Jenkins and JIRA as solutions that allow for orchestration of testing with minimal overhead. This will only be the first step towards the decomposition of this solution into one that can be designed for maximum efficiency and ease of management.

The final installment of this post will be a discussion regarding the transition of these services to one that is at least partially serverless. [Serverless computing][serverless-computing] is an execution model that cloud providers have popularized recently. It's a model that allows for focus on development of applications rather than the infrastructure supporting them, and low to no idle resource costs.

I hope you stick around for it!

<div style="float:right"><a href="{% post_url 2019-03-27-serverless-testing-orchestration-part-two %}">Part Two</a></div>

[selenium-grid]: https://www.seleniumhq.org/docs/07_selenium_grid.jsp
[cucumber-homepage]: https://cucumber.io/
[selenium-homepage]: https://www.seleniumhq.org/
[cucumber-photo]: https://images.pexels.com/photos/37528/cucumber-salad-food-healthy-37528.jpeg "Cucumber"
[behavior-driven-development]: https://en.wikipedia.org/wiki/Behavior-driven_development
[serverless-computing]: https://en.wikipedia.org/wiki/Serverless_computing
