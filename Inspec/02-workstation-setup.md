---
layout: tutorial
title: Workstation Setup
page-type: tutorial
tutorial-category: inspec
tutorial-order: 02
permalink: /inspec/workstation-setup
---

The following will guide you through how to get InSpec set up on a Windows system.

<h3>Install Chocolatey</h3>
For this tutorial you are going to install software by using the [Chocolatey](https://chocolatey.org/) package manager. In order to install Chocolatey, run the below code snippet in a PowerShell window.

```
Set-ExecutionPolicy RemoteSigned;
iwr https://chocolatey.org/install.ps1 -UseBasicParsing | iex
```
Note: you may have to close and reopen your Powershell window before using Chocolatey.

<h3>Install Ruby</h3>
Once you have Chocolatey installed you can use it to install Ruby. To do this run the below code snippet in a PowerShell window.

```
choco install ruby
```

<h3>Install InSpec</h3>
Once you have Ruby installed, you can use it to install the InSpec gem. To do this run the below code snippet in a PowerShell window.

Note: you may have to close and reopen your Powershell window before using the `gem` command.
```
gem install inspec
```

Note: you may have to close and reopen your Powershell window before using the InSpec gem.

After you have installed InSpec, open a new PowerShell window, type `inspec` and press enter. You should get output that looks similar to the below snippet, you have installed InSpec successfully.

```
Commands:
  inspec archive PATH                # archive a profile to tar.gz (default) ...
  inspec artifact SUBCOMMAND ...     # Sign, verify and install artifacts
  inspec check PATH                  # verify all tests at the specified PATH
  inspec compliance SUBCOMMAND ...   # Chef Compliance commands
  inspec detect                      # detect the target OS
  inspec env                         # Output shell-appropriate completion co...
  inspec exec PATHS                  # run all test files at the specified PATH.
  inspec habitat SUBCOMMAND ...      # Commands for InSpec + Habitat Integration
  inspec help [COMMAND]              # Describe available commands or one spe...
  inspec init TEMPLATE ...           # Scaffolds a new project
  inspec json PATH                   # read all tests in PATH and generate a ...
  inspec shell                       # open an interactive debugging shell
  inspec supermarket SUBCOMMAND ...  # Supermarket commands
  inspec vendor PATH                 # Download all dependencies and generate...
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
In order to write and edit InSpec code, you'll need to use a text editor. While the default Notepad editor on Windows will work, I would recommend you use a text editor that is more robust. The two I recommend are [Visual Studio Code](https://code.visualstudio.com/) or [Atom](https://atom.io/). Both are free and open source and you can install either of these with Chocolatey.

To install Visual Studio Code, run the following inside a PowerShell window:
```
choco install visualstudiocode
```

To install Atom, run the following inside a PowerShell window:
```
choco install Atom
```

<h3>Install ConEmu (Optional)</h3>
I personally use [ConEmu](https://conemu.github.io/) which is a console emulator for Windows. It isn't required to complete any of the tutorials in this tutorial series, but I personally use ConEmu to run my PowerShell instances. It has some handy features such as support for multiple tabs and better copy/paste support.

To install ConEmu, run the following inside a PowerShell window:
```
choco install conemu
```
