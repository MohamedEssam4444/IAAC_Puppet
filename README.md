# IAAC using Puppet
controlling nodes using declarative language (puppet) to maintain services on remote servers and execute commands remotely to them by simulating nodes by using Dockeragents .
## Archeticture

![Blank diagram (2)](https://user-images.githubusercontent.com/68178003/101762719-74b77000-3ae6-11eb-91a1-632409354194.jpeg)

* **Puppet Master**:
Puppet Master is the key mechanism which handles all the configuration related stuff. It applies the configuration to nodes using the Puppet agent. recieve facts from agents and process them and send them back as catalog which is rules given to agent which then given to package providers to execute these rules on agent pc .

* **Puppet Agent**:
Puppet Agents are the actual working machines which are managed by the Puppet master. They have the Puppet agent daemon service running inside them.

* **Config Repository**:
This is the repo where all nodes and server-related configurations are saved and pulled when required.

* **Facts**:
Facts are the details related to the node or the master machine, which are basically used for analyzing the current status of any node. On the basis of facts, changes are done on any target machine. There are pre-defined and custom facts in Puppet.

* **Catalog**:
All the manifest files or configuration which are written in Puppet are first converted to a compiled format called catalog and later those catalogs are applied on the target machine.
### installation :

* #Installing Puppet 
$ `wget http://puppetlabs.com/downloads/gems/puppet-0.25.1.gem `
$ `sudo gem install puppet-0.25.1.gem `
### Files & directories:
* **puppetfile**:
* Contain modules to be downloaded and installed
* A Puppetfile specifies detailed information about each environment's Puppet code and data, including where to get that code and data from, where to install it, and whether to update it.

* Both Code Manager and r10k use a Puppetfile to install and manage the content of your environments.
* In Puppet, we have a code management tool known as r10k that helps in managing environment configurations related to different kind of environments that we can configure in Puppet such as development, testing, and production. This helps in storing environment-related configuration in the source code repository. Using the source control repo branches, r10k creates environments on Puppet master machine installs and updates environment using modules present in the repo.

* Gem file can be used to install r10k on any machine.
* `gem install r10k`
* Configure environment in `/etc/puppet/puppet.conf`:
`[agent]` 
`server=master.puppet.vm` #when puppet agent runs the system will need to know what puppet server to point to
* Create a Configuration File for r10k Config
`cat <<EOF >/etc/r10k.yaml 
#The location to use for storing cached Git repos 
:cachedir: '/var/cache/r10k' 
#A list of git repositories to create 
:sources: 
#This will clone the git repository and instantiate an environment per
  #branch in /etc/puppetlabs/code/environments
  :my-org:
    remote: 'https://github.com/$YOUR_GITHUB_USERNAME$/control-repo.git'
    basedir: '/etc/puppetlabs/code/environments'
EOF`
* on the shell terminal: 
`r10 deploy environmnet -p`
* **environmnet.conf**: 
* Contain Modulepath
* This is one of the key settings in environment.conf file. All the directors defined in modulepath are by default loaded by Puppet. This is the path location from where Puppet loads its modules
* needs to explicitly set this up
* **Manifests**: 
* contain files that store roles we want to apply, those files end with .pp extension
* In puppet, all the programs are written in Ruby programming language and added with an extension of .pp is known as manifests. The full form of .pp is the puppet program.
* Manifest files are puppet programs. This is used to manage the target host system
    * **site.pp**: where puppet master first look for configurations, details about the system when the puppet agent checks in . 
    * contain node defininstions which define what classes(roles) will be included for what nodes 
    * here we specificy 4 roles 3 roles of them for specific nodes: for master.puppet.vm , and any node it's name end with web and any node end with db and the 4th rule which is for the default node is for any nodes other than those 3 specified
* **site**: 
* Directory containing role and profile
* Puppet relies mainly on **Abstraction** which are Resources to make system more manageable 
* There is a built in layer of abstraction where : Files , users , packages and services are abstracted into resources
* Modules are resources grouped together into custom resource type and class and bundeled together into modules
* Resources : single unit of configuration in puppet
* That layer of abstraction means you can provision a webserver without having to define all files , package , users that can be configured .
* Roles and Profiles:
bundle together specific configuration into more abstracted layer : **profile** and **roles** for keeping code organized
* **Profile**: 
* classes that group together subset of configurations
* for example: webserver would go to one profile and db to another profile
* **base**: profile included in all roles so each node has this profile, in this example it ensures that the user admin is present on each node as shown in the roles
* **agent_nodes**: deploy 2 docker agents named web and db on the puppet master which will then be include in the roles directory in the master role 
* **Roles**:
* Finally each machine will get assigned one role class which bundles up collections of profiles 
* class is a collection of puppet code that group together resources under a name that can be included in other code  
