---
layout: tutorial
title: Using the Registry Key
page-type: tutorial
tutorial-category: inspec
tutorial-order: 05
permalink: /inspec/reg-keys
---

As a Windows systems administrator, you've probably been in the registry more times than you can count. Luckily for us, there's a built in <a href="http://inspec.io/docs/reference/resources/registry_key/">registry key resource</a>. Before you continue with this tutorial, familiarize yourself with the `registry_key` resource.

Regisry keys are relitavely easy to audit with InSpec. Let's start with auditing the `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Class\{4D36E972-E325-11CE-BFC1-08002bE10318}` key. This is where network interface card (NIC) settings are stored in the registry. This resgistry key should always exist and you can audit for existence like this:

{% highlight ruby %}
describe registry_key('HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Class\{4D36E972-E325-11CE-BFC1-08002bE10318}') do
  it { should exist }
end
{% endhighlight %}

To start auditing property values of this key, you can start adding more criteria to this test. To check the `Class` property of this key, you can add the following:
```
its('Class') { should eq 'Net' }
```

Or, to make sure the `LowerLogoVersion` is greater than or equal to a certain value, you can add the following:
```
its('LowerLogoVersion') { should be >= 6.0 }
```

Now, your registry key test looks like this:
{% highlight ruby %}
describe registry_key('HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Class\{4D36E972-E325-11CE-BFC1-08002bE10318}') do
  it { should exist }
  its('Class') { should eq 'Net' }
  its('LowerLogoVersion') { should be >= 6.0 }
end
{% endhighlight %}

We could keep going and keep adding more criteria to this test, but you get the point. For more examples on different ways to use the `registry_key` resource, hit up the official documentation above. Now, let's go deeper into the `registry_key` resource.

<h3>Understanding the .children Method</h3>

Let's say you need to perform an audit on all of the NICs on the local system. Instead of writing the above resource for each of it's subkeys, you can utilize the `.children` method. What `.children` does is grabs every sub-key recursively underneath the specified registry path. So, it will grab sub-keys, sub-sub-keys, sub-sub-sub keys, an so forth. But, you only want to audit the direct sub-keys of `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Class\{4D36E972-E325-11CE-BFC1-08002bE10318}`. Here's a representation of what the tree structure of this registry key looks like:

```
- {4D36E972-E325-11CE-BFC1-08002bE10318}
  - 0000
  - 0001
  - 0002
  - 0003
  - 0004
  - Configuration
  - Properties
```
The NICs are represented by the keys that contain 4 numeric digits; these are the keys we'd like to audit. So, we need to come up with a regular expression that filters out all sub-keys as well as the `Configuration` and `Properties` keys.

To do this, you can run the following:

{% highlight ruby %}
describe registry_key('HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Class\{4D36E972-E325-11CE-BFC1-08002bE10318}').children(/08002bE10318\}\\[0-9]{4}$/).each { |key|
  describe registry_key(key) do
    it { should exist }
    # You can add additional creteria here
  end
{% endhighlight %}

What this test does is strips out all keys that don't end in `08002bE10318}\XXXX` replacing `XXXX` with numeric values only. Now, we can audit registry key values for each of the NICs on the system. This is incredibly handy especially if the number of NICs change from system to system.
