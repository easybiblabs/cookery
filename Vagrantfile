this_dir = File.dirname(__FILE__)
lib_dir = File.dirname(this_dir)

#require "#{lib_dir}/vagrant-test-library"
#vagrant_test_config = get_json(lib_dir + '/setup.json')

Vagrant::Config.run do |config|

  bibconfig = Bib::Vagrant::Config.new
  vagrantconfig = bibconfig.get

  config.vm.boot_mode = vagrantconfig['gui']
  config.vm.box = 'imagineeasy-ubuntu-14.04.3_virtualbox-4.3.26r98988_chef-11.10.4_1'

  config.vm.network :hostonly, '10.23.23.24'

  config.vm.customize [
    "modifyvm", :id,
    "--name", 'the ies cookery'
  ]

  config.vm.provision :shell, :inline => 'apt-spy2 fix --launchpad --country=de --commit'
  config.vm.provision :shell, :inline => 'apt-get update -y'
  config.vm.provision :shell, :inline => 'apt-get remove -y ruby1.9.1'
  config.vm.provision :shell, :inline => 'apt-get install -y ruby2.0-dev ruby2.0'
  config.vm.provision :shell, :inline => 'update-alternatives --install /usr/bin/ruby ruby /usr/bin/ruby2.0 1'
  config.vm.provision :shell, :inline => 'update-alternatives --install /usr/bin/gem gem /usr/bin/gem2.0 1'
  config.vm.provision :shell, :inline => 'cat /vagrant/gemrc > /root/.gemrc'
  config.vm.provision :shell, :privileged => false, :inline => 'cat /vagrant/gemrc > /home/vagrant/.gemrc'
  config.vm.provision :shell, :inline => 'apt-get install ruby-bundler'
  config.vn.provision :shell, :priviledge => false, :inline => 'cd /vagrant && bundle install'
end
