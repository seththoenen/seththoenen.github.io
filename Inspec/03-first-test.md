---
layout: tutorial
title: Writing and Running an Inspec Test
page-type: tutorial
tutorial-category: inspec
tutorial-order: 03
permalink: /inspec/first-test
---

Now that your workstation is set up, you are ready to crank out some Inspec tests. For this tutorial series, all example files will be placed inside a folder located at `C:\Inspec\`. 

Create a new file and call it `first-test.rb` inside the `C:\Inspec\` folder. Open the file with your text editor of choice. You are going to use the same test mentioned in the first tutorial for this example. Write the following code in your `first-test.rb` file.

{% highlight ruby %}
describe directory('C:\Windows') do
  it { should exist }
end
{% endhighlight %}

Save `first-test.rb` and open up a PowerShell window. Navitage to the `C:\Inspec\` directory and run the following command:

```
inspec exec .\first-test.rb
```

You should get output that looks like this:

```
C:\Inspec> inspec exec .\first-test.rb

Target:  local://


  File C:\Windows
     ✔  should exist

Test Summary: 1 successful, 0 failures, 0 skipped
```

Congratulations for writng and running your first Inspec test! Here is a breakdown of what happened.

The `inspec` command is primary method to do anything Inspec related on your local machine. You will be using this command a lot in this tutorial series. The `exec` sub-command tells Inspec to execute the file(s) given in the next argument. You gave the `exec` sub-command the file you just created, `first-test.rb`. So, what happened is you told Inspec to execute the contents of `first-test.rb`.

If you'd like to know more about the `exec` sub-command, you can run the following and it will give you all of the options and argument for `exec`.

```
inspec help exec
```
