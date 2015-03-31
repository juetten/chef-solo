# -*- mode: ruby -*-
# vi: set ft=ruby :
# vi: set sw=2 : 
# vi: set ts=2 :

require 'chef'
require 'json'
			

Chef::Config.from_file(File.join(File.dirname(__FILE__), '.chef', 'knife.rb'))
vagrant_json = JSON.parse(Pathname(__FILE__).dirname.join('nodes', (ENV['NODE'] || 'web1.example.com.json')).read)

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
			
		config.vm.box = "precise64"
		config.vm.network :forwarded_port, guest: 80, host: 8085 # <== add port forwarding
		config.vm.provision :chef_solo do |chef|
		chef.cookbooks_path = Chef::Config[:cookbook_path]
		chef.roles_path = Chef::Config[:role_path]
		chef.data_bags_path = Chef::Config[:data_bag_path]

		chef.environments_path = Chef::Config[:environment_path]
		chef.environment = ENV['ENVIRONMENT'] || 'development'

		chef.run_list = vagrant_json.delete('run_list')
		chef.json = vagrant_json
  end
end
