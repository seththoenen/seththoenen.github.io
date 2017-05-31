---
layout: tutorial
title: Getting Started with InSpec on Microsoft Azure 
page-type: tutorial
tutorial-category: cloud-inspec
tutorial-order: 01
permalink: /cloud-inspec/getting-started-azure
---

Author: Seth Thoenen

Co-Author: Shane Chaplain

Date: 5/28/2017

This is a comprehensive tutorial that will walk you through connecting your local Windows 10 machine to Microsoft Azure so you can start writing InSpec tests. This tutorial will utilize the [inspec-azure](https://github.com/chef/inspec-azure) repository announced at ChefConf 2017. Eventually, this will be merged into the base InSpec gem.

If you haven't ever written InSpec before, you will need to [set up your workstation](/inspec/workstation-setup) before starting this tutorial.

### 1. Generate a New InSpec Profile

Navigate to a folder where you would like to place your InSpec profile. This tutorial will put it on the root of the C:\ drive. To create a new InSpec profile, run the following command from PowerShell. Be sure to change directory into the directory you want your profile to be in.

```
inspec init profile azure-inspec-profile
```

From here, `cd` into your newly created InSpec profile.

```
cd .\azure-inspec-profile
```

Open up the entire profile in a code editor such as Atom or Visual Studio Code. If you have Visual Studio Code installed, you can open it from the command line with this command. You must be inside your InSpec profile directory for this to work.

```
code .
```

### 2. Update InSpec.yml

The `inspec-azure` repository contains an additional set of controls that add functionality to the base InSpec gem. In order to use them in your InSpec profile, you will need to create a dependency so your InSpec profile references the controls in the `inspec-azure` repository. You can do this by updating `inspec.yml` and appending the following code:

```
depends:
  - name: azure
    url: https://github.com/chef/inspec-azure/archive/0.6.1.tar.gz
```

This will use version 0.6.1 of the `inspec-azure` repository which was the latest version at the time of this writing. If you would like to use the latest version of the `inspec-azure` repository, replace the above URL with this one.

```
https://github.com/chef/inspec-azure/archive/master.tar.gz
```

### 3. Install Ruby Gem Dependencies

The `inspec-azure` repository utilizes several ruby gems under the hood that aren't included with the InSpec gem yet. Eventually, these will most likely be added as a dependency for the InSpec gem. But, for now, we need to install them individually. Run the following command to install the gems required by `inspec-azure`.

```
gem install ms_rest_azure azure_mgmt_resources azure_mgmt_compute azure_mgmt_network inifile
```

### 4. Check the InSpec Profile

InSpec has a built in tool that allows you to check your profile before you execute it. Do this now to see if there are any errors with your profile's configuration.

```
inspec check .
```

Note: You must be inside the InSpec profile's directory for this to work.

If everything is configured properly, you should get output that looks like this.

```
Location:    .
Profile:     azure-inspec-profile
Controls:    2
Timestamp:   2017-05-29T08:48:07-05:00
Valid:       true
```

If you get a stacktrace, there are errors that must be corrected.

### 5. Configure Microsoft Azure

Now that you have an InSpec profile configured to use the `inspec-azure` extension, you need to create an API so InSpec can interface with Microsoft Azure and gather the credentials needed to authenticate with Microsoft Azure. First, create a file in your home directory to hold these credentials.

```
New-Item c:\users\[USER NAME]\.azure -type directory
New-Item c:\users\[USER NAME]\.azure\credentials -type file
```

Open the `credentials` file, and populate it with the following contents.

```
[<SUBSCRIPTION_ID>]
client_id = "<CLIENT_ID>"
client_secret = "<CLIENT_SECRET>"
tenant_id = "<TENANT_ID>"
```

Now, you need to gather the required credentials to connect to Microsoft Azure and set up an API for InSpec scans.

Refer to the following guide to get your `SUBSCRIPTION_ID`: [Getting your Azure Subscription GUID](https://blogs.msdn.microsoft.com/mschray/2016/03/18/getting-your-azure-subscription-guid-new-portal/)

Refer to the following guide to get your `CLIENT_ID`, `CLIENT_SECRET`, and to set up an API for InSpec scans: [Use portal to create an Azure Active Directory application and service principal that can access resources](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-create-service-principal-portal)

To get your `TENANT_ID`, perform the following steps in the Microsoft Azure Portal.

1. Click `Azure Active Directory` in the main left navigation menu.

2. Click `Properties`.

3. The field labeled `Directory ID` is your `TENANT_ID`.

Once you've gathered all of your credential information and created the necessary API, populate your credentials file. It should look something like this.

```
[12345678-90ab-cdef-1234-567890abcdef]
client_id = "12345678-90ab-cdef-1234-567890abcdef"
client_secret = "2eNL2hZNSjufOMN4Kj7yueeKYrpcQqkCRZe0BBJpZfP="
tenant_id = "12345678-90ab-cdef-1234-567890abcdef"
```

### 6. Install Azure Resource manager

There are PowerShell dependencies built into `inspec-azure` that require certain PowerShell libraries to exist on your local machine. To ensure they are installed, run the following command.

```
Install-Module AzureRM
```

Answer `[Y] Yes` or `[A] Yes to All` when prompted.

### 7. Begin Auditing

Now, you have all of the pieces necessary to start auditing your Microsoft Azure instance! A following tutorial will be written that goes into detail of how to use the built in controls for auditing Azure. But, for now, you update the `example.rb` file inside the `controls` folder to include the following, substituting your information where appropriate.

```
control 'azure-1' do
  impact 1.0
  title 'Make sure Ubuntu Servers are built from an Ubuntu template'
  describe azure_virtual_machine(name: '[YOUR VM NAME]', resource_group: '[YOUR RESOURCE GROUP]') do
    its('sku') { should eq '16.04.0-LTS' }
    its('publisher') { should eq 'Canonical' }
    its('offer') { should eq 'UbuntuServer' }
  end
end
```

To execute this test against your Azure instance, run the following command.

```
inspc exec .
```

And you should get output that looks similar to this.

```
Profile: InSpec Profile (azure-inspec-profile)
Version: 0.1.0
Target:  local://

  [CRIT]  azure-1: Make sure Ubuntu Servers are built from an Ubuntu template (3 failed)
     [FAIL]  azure_virtual_machine sku should eq "16.04.0-LTS"

     expected: "16.04.0-LTS"
          got: "7.2n"

     (compared using ==)

     [FAIL]  azure_virtual_machine publisher should eq "Canonical"

     expected: "Canonical"
          got: "OpenLogic"

     (compared using ==)

     [FAIL]  azure_virtual_machine offer should eq "UbuntuServer"

     expected: "UbuntuServer"
          got: "CentOS"

     (compared using ==)



Profile: Azure Resource Pack (azure)
Version: 0.1.0
Target:  local://

     No tests executed.

Profile Summary: 0 successful, 1 failures, 0 skipped
Test Summary: 0 successful, 3 failures, 0 skipped
```