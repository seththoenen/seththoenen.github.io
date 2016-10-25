---
layout: tutorial
title: Introducing Controls
page-type: tutorial
tutorial-category: inspec
tutorial-order: 04
permalink: /inspec/controls
---

Now that you've written your first test, lets dive into what a controls is. Think of a control as a container for tightly coupled InSpec tests. For example, if you wanted to perform an audit to make sure you adhere by a particular CIS standard you could create a control and populate it with any tests related to that CIS standard.

Here's an example of what an InSpec control looks like:

{% highlight ruby %}
control 'test_exists' do # This is the name of the test.
  impact 1.0 # You can give it an impact score. This is used for Chef Complaince. Ranges are from 0.0 - 10.0
  title 'Example InSpec control that audits a file and an folder' # This is the title of the control.
  desc '
    You can put an advanced, multi-
    line description here if you want.
  '
  describe directory('c:\users\sthoenen\desktop\test') do # this is where the test begins
    it { should exist } # criteria for the test
  end

  describe file('c:\users\sthoenen\desktop\test\test.txt') do # A second test for this control
    it { should exist } 
  end
end
{% endhighlight %}

What the above control does is verifies that a folder called `test` exists on the Administrator's desktop and that a file named `test.text` exists inside of it. These two tests are bundled together in an InSpec control. A control should have at least one InSpec test.

Run the above code snippet on your local machine to see what audit failures look like. You will need to replace `administrator` with your user account name. Create the folder and text file to see how partial InSpec contorls react under differing conditions.
