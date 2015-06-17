# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|
  if Vagrant.has_plugin?('vagrant-cachier')
    config.cache.scope = :box
  else
    STDERR.puts "Install plugin:\n$ vagrant plugin install vagrant-cachier"
  end

  if Vagrant.has_plugin?('vagrant-omnibus')
    config.omnibus.chef_version = '12.3.0'
  end

  config.vm.box = 'boxcutter/ubuntu1404'

  config.berkshelf.enabled = true

  config.vm.provision :chef_solo do |chef|
    chef.formatter = ENV.fetch('CHEF_FORMAT', 'null').downcase.to_sym
    chef.log_level = ENV.fetch('CHEF_LOG', 'info').downcase.to_sym

    chef.json = {
      "rackspace_monitoring" => {
        "cloud_credentials_username" => ENV['RACKSPACE_USERNAME'],
        "cloud_credentials_api_key" => ENV['RACKSPACE_API_KEY']
      },
      "cloud" => {
        "local_ipv4" => "10.0.0.1",
        "public_ipv4" => "8.0.0.1"
      }
    }

    chef.run_list = %w(
      recipe[rackspace_monitoring_service_test::default]
      recipe[rackspace_monitoring_check_test::cpu]
      recipe[rackspace_monitoring_check_test::disk]
      recipe[rackspace_monitoring_check_test::filesystem]
      recipe[rackspace_monitoring_check_test::http]
      recipe[rackspace_monitoring_check_test::load]
      recipe[rackspace_monitoring_check_test::memory]
      recipe[rackspace_monitoring_check_test::network]
      recipe[rackspace_monitoring_check_test::apache]
      recipe[rackspace_monitoring_check_test::mysql]
      recipe[rackspace_monitoring_check_test::redis]
      recipe[rackspace_monitoring_check_test::custom]
      recipe[rackspace_monitoring_check_test::plugin]
    )
  end
end
