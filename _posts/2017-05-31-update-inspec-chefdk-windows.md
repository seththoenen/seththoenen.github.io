---
layout: post
title:  "Update ChefDK's InSpec Version on Windows"
date:   2017-05-31 17:00:25 -0500
categories: post
---

Let's assume there's a new version of InSpec that just dropped and you really want to use it, but you are on Windows and your system is configured to use the InSpec version that ships with the ChefDK. Well, first you need to update the Chef gem by running `chef gem update inspec`. Do this and you'll get output that looks like this:

```
Updating installed gems
Updating inspec
Fetching: train-0.24.0.gem (100%)
Successfully installed train-0.24.0
Fetching: inspec-1.26.0.gem (100%)
Successfully installed inspec-1.26.0
Parsing documentation for train-0.24.0
Installing ri documentation for train-0.24.0
Installing darkfish documentation for train-0.24.0
Parsing documentation for inspec-1.26.0
Installing ri documentation for inspec-1.26.0
Installing darkfish documentation for inspec-1.26.0
Done installing documentation for train, inspec after 31 seconds
Parsing documentation for inspec-1.26.0
Parsing documentation for train-0.24.0
Done installing documentation for inspec, train after 5 seconds
Gems updated: inspec train
```

Well, everything looks good *taps easy button*, and it looks like `train` updated as well, cool. Let's check your InSpec version by running `inspec version` and you will probably be disappointed to see something like this:

```
1.25.1

Your version of InSpec is out of date! The latest version is 1.26.0.
```

But, you just installed 1.26.0. What's happening is there's a file located at `C:\opscode\chefdk\bin\inspec` (not `inspec.bat`, the one without a file extension) that specifies which version of InSpec and it's dependencies to load. Open this file with your text editor of choice and bump the inspec values to your desired version. In this case, version 1.26.0. You'll have to do this in two locations in this file. It should look like this:

```
gem "inspec", "= 1.26.0"

spec = Gem::Specification.find_by_name("inspec", "= 1.26.0")
```

However, remember that `train` also updated. Update the `train` value to the version that updated as well (in this case 0.24.0)

Cool, now lets check your InSpec version again:

```
1.26.0
```

Now, brush your shoulders off and InSpec all the things.
