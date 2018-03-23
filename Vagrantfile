#ENV['VAGRANT_DEFAULT_PROVIDER'] = 'docker'

Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/trusty64"
  config.vm.synced_folder "../microservice-demo", "/microservice-demo", create: true
  #config.vm.network "forwarded_port", guest: 8080, host: 18080
  #config.vm.network "forwarded_port", guest: 8761, host: 18761
  #config.vm.network "forwarded_port", guest: 8989, host: 18989
  config.vm.provider "virtualbox" do |v|
    v.memory = 1024
    v.cpus = 1
  end
#  config.vm.provider "docker" do |d|
  config.vm.provision "docker" do |d|
    d.build_image "/vagrant/hateoas", args: "-t hateoas"
    d.build_image "/vagrant/haproxy", args: "-t haproxy"
    #d.run "hateoas1", image: "hateoas"
    #d.run "hateoas2", image: "hateoas"
    #d.run "haproxy", image: "haproxy"
  end
end
