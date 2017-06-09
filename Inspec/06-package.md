---
layout: tutorial
title: Targeting Packages
page-type: tutorial
tutorial-category: inspec
tutorial-order: 06
permalink: /inspec/package
---

InSpec's `package` resource allows audits of packages installed on the system. The official InSpec documentation provides the following example:

{% highlight ruby %}
describe package('name') do
  it { should be_installed }
end
{% endhighlight %}

This is pretty straightforward. Just replace `name` with any installed program that's listed in Programs and Features on the system you would like to audit. However, not all installed packages show up in Programs and Features. The `package` resource works by parsing information from the following registry key locations:

<ul>
  <li>HKEY_LOCAL_MACHINE:\SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall\</li>
  <li>HKEY_CURRENT_USER:\SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall\</li>
  <li>HKEY_LOCAL_MACHINE:\SOFTWARE\Wow6432Node\Microsoft\Windows\CurrentVersion\Uninstall\</li>
  <li>HKEY_CURRENT_USER:\SOFTWARE\Wow6432Node\Microsoft\Windows\CurrentVersion\Uninstall\</li>
</ul>

Any key in the paths above that has a `DisplayName` property can be targeted with the `package` resource. The only other item you can audit with `package` is the package's version.

For example, if you have Visual Studio Code Installed, you can find it's registry key here: `HKEY_LOCAL_MACHINE:\SOFTWARE\Wow6432Node\Microsoft\Windows\CurrentVersion\Uninstall\{F8A2A208-72B3-4D61-95FC-8A65D340689B}_is1` 

It should have some properties that look like this:

<table>
  <tr>
    <th>Name</th>
    <th>Type</th>
    <th>Data</th>
  </tr>
  <tr>
    <td>DisplayName</td>
    <td>REG_SZ</td>
    <td>Microsoft Visual Studio Code</td>
  </tr>
  <tr>
    <td>DisplayVersion</td>
    <td>REG_SZ</td>
    <td>1.7.1</td>
  </tr>
</table>

An example of an audit for the Visual Studio Code package could look like this:

{% highlight ruby %}
describe package('Microsoft Visual Studio Code') do
  it { should be_installed }
  its('version') { should be >= '1.0.0'}
end
{% endhighlight %}

Alternatively, you can ensure a package isn't installed. To do this, simply use `it { should_not be_installed }` like this:

{% highlight ruby %}
describe package('emacs') do
  it { should_not be_installed }
end
{% endhighlight %}
