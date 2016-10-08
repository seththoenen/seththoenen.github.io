---
layout: tutorial
title: Workstation Setup
page-type: tutorial
tutorial-category: inspec
tutorial-order: 02
permalink: /inspec/workstation-setup
---

The following will guide you through how to get Inspec set up on a Windows system.

<h3>Install Chocolatey</h3>
For this tutorial you are going to install software by using the [Chocolatey](https://chocolatey.org/) package manager. In order to install Chocolatey, run the below code snippet in a PowerShell window.

```
Set-ExecutionPolicy RemoteSigned;
iwr https://chocolatey.org/install.ps1 -UseBasicParsing | iex
```
Note: you may have to close your Powershell window before using Chocolatey.

<h3>Install Ruby</h3>
Once you have Chocolatey installed you can use it to install Ruby. To do this run the below code snippet in a PowerShell window.

```
choco install Ruby
```

<h3>Install Inspec</h3>
In order to install Inspec, you will have to download it from the official [Inspec download page](https://downloads.chef.io/inspec/windows/). Download the Inspec installer and run it to install Inspec on your local machine.

After you have instaled Inspec, open a new PowerShell window, type `inspec` and press enter. If you are presented with an error message, you haven't installed Inspec. Otherwise, if you get output that looks similar to the below snippet, you have installed Inspec successfully.

```
Commands:
  inspec archive PATH                # archive a profile to tar.gz (default) ...
  inspec check PATH                  # verify all tests at the specified PATH
  inspec compliance SUBCOMMAND ...   # Chef Compliance commands
  inspec detect                      # detect the target OS
  inspec env                         # Output shell-appropriate completion co...
  inspec exec PATHS                  # run all test files at the specified PATH.
  inspec help [COMMAND]              # Describe available commands or one spe...
  inspec init TEMPLATE ...           # Scaffolds a new project
  inspec json PATH                   # read all tests in PATH and generate a ...
  inspec shell                       # open an interactive debugging shell
  inspec supermarket SUBCOMMAND ...  # Supermarket commands
  inspec vendor                      # Download all dependencies and generate...
  inspec version                     # prints the version of this tool

Options:
  l, [--log-level=LOG_LEVEL]         # Set the log level: info (default), debug, warn, error
      [--log-location=LOG_LOCATION]  # Location to send diagnostic log messages to. (default: STDOUT or STDERR)
      [--diagnose], [--no-diagnose]  # Show diagnostics (versions, configurations)
```

<h3>Install Git (Optional)</h3>
If you aren't familiar with [Git](https://git-scm.com/) or version control, I highly recommend you pause this tutorial now and read up on both of those topics. Using version control is a a good habit to get into, especially if you will be collaborating with team members.

Later tutorials may require you to clone code repositories from [GitHub](www.github.com). To install git now, run the following code in a PowerShell window.

```
choco install git
```

<h3>Install a Text Editor (Optoinal)</h3>
In order to write and edit Inspec code, you'll need to use a text editor. While the default Notepad editor on Windows will work, I would recommend you use a text editor that is more robust. The two I recommend are [Visual Studio Code](https://code.visualstudio.com/) or [Atom](https://atom.io/). Both are free and open source and you can install either of these with Chocolatey.

To install Visual Studio Code, run the following inside a PowerShell window:
```
choco install visualstudiocode
```

To install Atom, run the following inside a PowerShell window:
```
choco install Atom
```

<h3>Install ConEmu (Optional)</h3>
I personally use [ConEmu](https://conemu.github.io/) which is a console emulator for Windows. It isn't rquired to complete any of the tutorials in this tutorial series, but I personally use ConEmu to run my PowerShell instances. It has some handy features such as support for multiple tabs and better copy/paste support.

To install ConEmu, run the following inside a PowerShell window:
```
choco install conemu
```
