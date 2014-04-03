# What is OSIO Vagrant

This is a repository that provides a Vagrant-based development
environment for [OpenSensors](http://opensensors.io). It is heavily
inspired by (and borrows from)
[Sous Chef](https://github.com/michaelklishin/sous-chef/).

[Chef](http://www.opscode.com/chef/) is used for provisioning.


## Dependencies

You'll need [Vagrant](http://vagrantup.com) and [Virtual
Box](http://virtualbox.org) being installed. Note that you do not need
to have Chef installed locally. It will only be run in a virtual
machine.


## Getting Started

 * [Install VirtualBox](https://www.virtualbox.org/wiki/Downloads)
 * [Install Vagrant](http://www.vagrantup.com/downloads.html)
 * Install VirtualBox guest addition updater: `vagrant gem install vagrant-vbguest`

Copy sample Vagrant file:

    cp Vagrantfile.sample Vagrantfile

Clone [OpenSensors cookbooks](https://github.com/opensensorsIO/cookbooks):

    git clone git://github.com/opensensorsIO/cookbooks.git cookbooks

To allow provisioning with chef you need to uncomment the following line
(including its associated end statement)

      config.vm.provision :chef_solo do |chef|


After that point Vagrant at the cookbooks location by editing Vagrantfile. For Travis CI cookbooks, you just need to uncomment

    # this assumes you have opensensors/cookbooks cloned at ./cookbooks
    chef.cookbooks_path = ["cookbooks"]

You can use multiple cookbook locations if necessary.

Next choose some cookbooks to provision. In the case of OSIO
cookbooks, build-essential is a good one to start with, so uncomment

    # chef.add_recipe     "build-essential" 

Your Vagrantfile then will look like this:

    Vagrant::Config.run do |config|
      config.vm.box     = "precise32_base"
      config.vm.box_url = "http://cloud-images.ubuntu.com/vagrant/raring/current/raring-server-cloudimg-amd64-vagrant-disk1.box"
    
      config.vm.provision :chef_solo do |chef|
        # point Vagrant at the location of cookbooks you are going to use,
        # for example, a clone of your fork of github.com/travis-ci/travis-cookbooks
         chef.cookbooks_path = ["cookbooks"]
    
        # Turn on verbose Chef logging if necessary
        # chef.log_level      = :debug
    
        # List the recipies you are going to work on/need.
        chef.add_recipe     "build-essential"    
        # chef.add_recipe     "networking_basic"    
    
        # chef.json.merge!({
        #   :cookbook => {
        #     :user     => "vagrant",
        #     :group    => "vagrant"
        #   }
        # })
      end
    end

Create a virtual machine you will be developing cookbooks in:

    vagrant up 

To provision the VM ("provisioning" means running chef-solo to
converge the VM to the state you want using Chef recipes you'd chosen
to use):

    vagrant provision


Running chef-solo may take from several seconds to several minutes,
dependeing on what selected recipes do.

Once provisioning finishes, ssh into the VM to check what the
environment looks like:

    vagrant ssh

When you are done with your work on the cookbook you can either power
off the VM to use it later with

    vagrant halt

or destroy it completely with

    vagrant destroy


## Copyright

Michael S. Klishin & OpenSensors Development Team, 2014


## License

The MIT license. See LICENSE file in the repository root.
