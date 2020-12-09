# IAAC using Puppet
controlling nodes using declarative language (puppet) to maintain services on remote servers and execute commands remotely to them using client-server archeticture .
## Archeticture

![image](https://user-images.githubusercontent.com/68178003/101695940-ff17b980-3a7d-11eb-921b-fb5bde726b85.png)

* Puppet Master
Puppet Master is the key mechanism which handles all the configuration related stuff. It applies the configuration to nodes using the Puppet agent. recieve facts from agents and process them and send them back as catalog which is rules given to agent which then given to package providers to execute these rules on agent pc .

* Puppet Agent
Puppet Agents are the actual working machines which are managed by the Puppet master. They have the Puppet agent daemon service running inside them.

* Config Repository
This is the repo where all nodes and server-related configurations are saved and pulled when required.

* Facts
Facts are the details related to the node or the master machine, which are basically used for analyzing the current status of any node. On the basis of facts, changes are done on any target machine. There are pre-defined and custom facts in Puppet.

* Catalog
All the manifest files or configuration which are written in Puppet are first converted to a compiled format called catalog and later those catalogs are applied on the target machine.

### Files & directories:
**puppetfile**:
**environmnet.conf**: 
* Contain Modulepath
* This is one of the key settings in environment.conf file. All the directors defined in modulepath are by default loaded by Puppet. This is the path location from where Puppet loads its modules
* needs to explicitly set this up
**Manifests**: 
* contain files that store roles we want to apply, those files end with .pp extension
* In puppet, all the programs are written in Ruby programming language and added with an extension of .pp is known as manifests. The full form of .pp is the puppet program.
* Manifest files are puppet programs. This is used to manage the target host system
    **site.pp**:
    * contain node defininstions which define what classes(roles) will be included for what nodes 
    * here we specificy 4 roles 3 roles of them for specific nodes: for master.puppet.vm , and any node it's name end with web and any node end with db and the 4th rule which is for the default node is for any nodes other than those 3 specified
**site**: 
* Directory containing role and profile
* Puppet relies mainly on **Abstraction** which are Resources to make system more manageable 
* There is a built in layer of abstraction where : Files , users , packages and services are abstracted into resources
* Modules are resources grouped together into custom resource type and class and bundeled together into modules
* Resources : single unit of configuration in puppet
* That layer of abstraction means you can provision a webserver without having to define all files , package , users that can be configured .
* Roles and Profiles:
bundle together specific configuration into more abstracted layer : **profile** and **roles** for keeping code organized
**Profile**: 
* classes that group together subset of configurations
* for example: webserver would go to one profile and db to another profile
**Roles**:
* Finally each machine will get assigned one role class which bundles up collections of profiles 
* class is a collection of puppet code that group together resources under a name that can be included in other code  
