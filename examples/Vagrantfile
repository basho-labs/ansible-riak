# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"

# Ensure basho.riak-common dependency is in place
if [ "up", "provision" ].include?(ARGV.first) && File.directory?("roles") &&
  !(File.directory?("roles/basho.riak-common") || File.symlink?("roles/basho.riak-common"))
  system("ansible-galaxy install -r roles.txt -p roles --ignore-errors")
end

# Grab local IP address for the proxy
def local_ip
  @local_ip ||= begin
    orig, Socket.do_not_reverse_lookup = Socket.do_not_reverse_lookup, true

    UDPSocket.open do |s|
      s.connect "64.233.187.99", 1
      s.addr.last
    end
  ensure
    Socket.do_not_reverse_lookup = orig
  end
end

$platform = ENV["ANSIBLE_RIAK_OS"]

Vagrant.configure(VAGRANTFILE_API_VERSION) do |cluster|
  
  cluster.vm.box = if $platform == "UBUNTU"
    "chef/ubuntu-12.04"
  elsif $platform == "CENTOS"
    "chef/centos-6.5"
  elsif $platform == "DEBIAN"
    "chef/debian-7.4"
  else
    "chef/ubuntu-12.04"
  end

  # Wire up the proxy
  if Vagrant.has_plugin?("vagrant-proxyconf")
    begin
      TCPSocket.open('localhost', 8123) do |s|
        s.select()
      end
    rescue Exception=> e
       abort("proxy enabled, and expected by VM, but not running")
    end
    cluster.proxy.http     = "http://#{local_ip}:8123/"
    cluster.proxy.https    = "http://#{local_ip}:8123/"
    cluster.proxy.no_proxy = "localhost,127.0.0.1"
  end

  (6..8).each_with_index do |last_octet, index|
    index = index + 1

    cluster.vm.define "riak-0#{index}" do |machine|
      machine.vm.hostname = "riak-0#{index}"
      machine.vm.network "private_network", ip: "10.42.0.#{last_octet}"

      if ENV["ANSIBLE_RIAK_CS_FORWARD_PORTS"]
          puts machine.vm.hostname + ": Forwarding ports to local host" 
          machine.vm.network "forwarded_port", guest: 8087, host: (10000 * index) + 8087
          machine.vm.network "forwarded_port", guest: 8098, host: (10000 * index) + 8098
      end

      if (index) == 3
        machine.vm.provision "ansible" do |ansible|
          ansible.playbook = "./setup_riak.yml"
          ansible.inventory_path = "./hosts"
          ansible.raw_arguments = [ "--timeout=60" ]

          ansible.extra_vars = {
            riak_iface: "eth1",
          }

          ansible.verbose = "vvvv"
          ansible.limit = "all"
        end
      end
    end
  end
end
