---
layout: tutorial
title: What is Inspec?
page-type: tutorial
tutorial-category: inspec
tutorial-order: 01
permalink: /inspec/what-is-inspec
---
[Inspec](http://inspec.io) is an open source auditing framework that is writtein in Ruby and is maintained by [Chef](http://chef.io). Inspec allows you to see inside your systems by writing simple audit tests that utilize [Inspec Resoures](http://inspec.io/docs/reference/resources/). These tests can be executed on your local machine or on a remote server. 

Inspec is an incredibly flexible framework. If there isn't an Inspec resource that fits your needs, you can write your own or you can utilize the PowerShell resource to execute custom PowerShell commands. 

Another benefit of Inspec is how easy it is to read and write. You don't need to be a programmer to interpret or write Inspec resources. For example, here's what a simple directory resource looks like.

{% highlight ruby %}
describe directory('C:\Windows') do
  it { should exist }
end
{% endhighlight %}

This resource simply states that the directory `C:\Windows\` should exist. That may not seem to be very exciting to you right now, but it will get to the more advanced and powerfull use cases in later tutorials.

So, why should you use Inspec? Even though you may not work on an information security team or an audit team, keeping  systems compliant is important especially if they are in environments that are audited frequently. You can also use Inspec tests to verify that Chef recipes you are writing had their desired effect. Or, if your organization has a set of standards that all systems must abide by, you can easily write those in Inspec and audit your own development or test environments to catch issues before they make it to production. The list of Inspec use cases is nearly endless.

Before proceeding to the next tutorial, I'd highly recommend you go through the official Inspec tutorial located at the official Inspec website: [http://inspec.io](http://inspec.io). Keep in mind that the official Inspec tutorial is designed for Linux systems and this tutorial set will focus on Windows systems.
