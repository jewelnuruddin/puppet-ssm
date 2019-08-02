# ref: https://app.vagrantup.com/puppetlabs
nodes = [
  { :hostname => 'centos70pe',   :ip => '192.168.1.2',  :box => 'puppetlabs/centos-7.2-64-puppet-enterprise' },
  { :hostname => 'centos70',     :ip => '192.168.1.3',  :box => 'puppetlabs/centos-7.2-64-puppet' },
  { :hostname => 'centos66',     :ip => '192.168.1.4',  :box => 'puppetlabs/centos-6.6-64-puppet' },
  { :hostname => 'ubuntu1604',   :ip => '192.168.1.5',  :box => 'puppetlabs/ubuntu-16.04-64-puppet' },
  { :hostname => 'ubuntu1404',   :ip => '192.168.1.6',  :box => 'puppetlabs/ubuntu-14.04-64-puppet' },
  { :hostname => 'ubuntu1204',   :ip => '192.168.1.7',  :box => 'puppetlabs/ubuntu-12.04-64-puppet' },
  { :hostname => 'debian8',      :ip => '192.168.1.8',  :box => 'puppetlabs/debian-8.2-64-puppet' },
]

Vagrant.configure("2") do |config|
  nodes.each do |node|
    config.vm.define node[:hostname] do |nodeconfig|
      nodeconfig.vm.box = node[:box]
      nodeconfig.vm.hostname = node[:hostname] + ".box"
      nodeconfig.vm.network :private_network, ip: node[:ip]

      memory = node[:ram] ? node[:ram] : 256;
      nodeconfig.vm.provider :virtualbox do |vb|
        vb.customize [
          "modifyvm", :id,
          "--cpuexecutioncap", "50",
          "--memory", memory.to_s,
        ]
      end
    end
  end

  config.vm.provision :shell do |shell|
    script = "mkdir -p /etc/puppetlabs/code/environments/production/modules/ssm;" +
    "cp -r /vagrant/* /etc/puppetlabs/code/environments/production/modules/ssm/;" +
    "puppet module --modulepath=/etc/puppetlabs/code/environments/production/modules/ install puppetlabs/concat --version=1.2.4;" +
    'export PATH=$PATH:/opt/puppetlabs/puppet/bin;' +
    'export MODULEPATH=/etc/puppetlabs/code/environments/production/modules/;' +
    "puppet apply --pluginsync --modulepath=$MODULEPATH \"$MODULEPATH\"ssm/tests/init.pp;"
    shell.inline = script
  end
end
