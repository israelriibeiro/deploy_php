#deploy_php

####Table of Contents

1. [Overview](#overview)
2. [Module Description](#module-description)
3. [Setup](#setup)
    * [Resources managed by deploy_php module](#resources-managed-by-deploy_php-module)
    * [Setup requirements](#setup-requirements)
    * [Beginning with module deploy_php](#beginning-with-module-deploy_php)
4. [Usage](#usage)
5. [Operating Systems Support](#operating-systems-support)
6. [Development](#development)

##Overview

This module installs, manages and configures deploy_php.

##Module Description

The module is based on **stdmod** naming standards version 0.9.0.

Refer to http://github.com/stdmod/ for complete documentation on the common parameters.

##Setup

###Resources managed by deploy_php module
* This module installs the deploy_php package
* Enables the deploy_php service
* Can manage all the configuration files (by default no file is changed)

###Setup Requirements
* PuppetLabs [stdlib module](https://github.com/puppetlabs/puppetlabs-stdlib)
* StdMod [stdmod module](https://github.com/stdmod/stdmod)
* Puppet version >= 2.7.x
* Facter version >= 1.6.2

###Beginning with module deploy_php

To install the package provided by the module just include it:

```puppet
     include deploy_php
```

The main class arguments can be provided either via Hiera (from Puppet 3.x) or direct parameters:

```puppet
     class { 'deploy_php':
       parameter => value,
     }
```

The module provides also a generic define to manage any deploy_php configuration file:

```puppet
     deploy_php::conf { 'sample.conf':
       content => '# Test',
     }
```

##Usage

* A common way to use this module involves the management of the main configuration file via a custom template (provided in a custom site module):

```puppet
     class { 'deploy_php':
       config_file_template => 'site/deploy_php/deploy_php.conf.erb',
     }
```

* You can write custom templates that use setting provided but the config_file_options_hash paramenter

```puppet
     class { 'deploy_php':
       config_file_template      => 'site/deploy_php/deploy_php.conf.erb',
       config_file_options_hash  => {
         opt  => 'value',
         opt2 => 'value2',
       },
     }
```

* Use custom source (here an array) for main configuration file. Note that template and source arguments are alternative.

```puppet
     class { 'deploy_php':
       config_file_source => [ "puppet:///modules/site/deploy_php/deploy_php.conf-${hostname}" ,
                               "puppet:///modules/site/deploy_php/deploy_php.conf" ],
     }
```

* Use custom source directory for the whole configuration directory, where present.

```puppet
     class { 'deploy_php':
       config_dir_source  => 'puppet:///modules/site/deploy_php/conf/',
     }
```

* Use custom source directory for the whole configuration directory and purge all the local files that are not on the dir.
  Note: This option can be used to be sure that the content of a directory is exactly the same you expect, but it is desctructive and may remove files.

```puppet
     class { 'deploy_php':
       config_dir_source => 'puppet:///modules/site/deploy_php/conf/',
       config_dir_purge  => true, # Default: false.
     }
```

* Use custom source directory for the whole configuration dir and define recursing policy.

```puppet
     class { 'deploy_php':
       config_dir_source    => 'puppet:///modules/site/deploy_php/conf/',
       config_dir_recursion => false, # Default: true.
     }
```

* Provide an hash of files resources to be created with deploy_php::conf.

```puppet
     class { 'deploy_php':
       conf_hash => {
         'deploy_php.conf' => {
           template => 'site/deploy_php/deploy_php.conf',
         },
         'deploy_php.other.conf' => {
           template => 'site/deploy_php/deploy_php.other.conf',
         },
       },
     }
```

* Do not trigger a service restart when a config file changes.

```puppet
     class { 'deploy_php':
       config_dir_notify => '', # Default: Service[deploy_php]
     }
```

##Operating Systems Support

This is tested on these OS:
- RedHat osfamily 5 and 6
- Debian 6 and 7
- Ubuntu 10.04 and 12.04

##Development

Pull requests (PR) and bug reports via GitHub are welcomed.

When submitting PR please follow these quidelines:
- Provide puppet-lint compliant code
- If possible provide rspec tests
- Follow the module style and stdmod naming standards

When submitting bug report please include or link:
- The Puppet code that triggers the error
- The output of facter on the system where you try it
- All the relevant error logs
- Any other information useful to undestand the context
