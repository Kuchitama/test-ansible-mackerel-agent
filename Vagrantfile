# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://vagrantcloud.com/search.
  config.vm.box = "stakahashi/amazonlinux2"

  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  config.vm.box_check_update = true

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  config.vm.network "private_network", ip: "192.168.33.10"

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  # config.vm.synced_folder "../data", "/vagrant_data"

  config.trigger.after :up do |trigger|
    trigger.info = "Change mackerel status to working after vagrant up"
    trigger.run_remote = { inline: "mkr status | /usr/bin/jq -r .id | xargs mkr update --status working" }
  end

  config.trigger.before :halt do |trigger|
    trigger.info = "Change mackerel status to maintenance before halt"
    trigger.run_remote = { inline: "mkr status | /usr/bin/jq -r .id | xargs mkr update --status maintenance" }
  end

  config.trigger.before :destroy do |trigger|
    trigger.info = "Retire from mackerel before destroy"
    trigger.run_remote = { inline: "mkr retire" }
  end

  config.vm.provision :ansible do |ansible|
    ansible.playbook = "vagrant.yml"
    ansible.compatibility_mode = "2.0"
  end
end
