#ENV['VAGRANT_DEFAULT_PROVIDER'] = 'docker'

Vagrant.configure("2") do |config|
  config.vm.define("hateoas1") do |hateoas1|
    hateoas1.vm.box = "centos/7"
    hateoas1.vm.network "private_network", ip: "192.168.15.21"
    #config.vm.synced_folder "../microservice-demo", "/microservice-demo", create: true
    #config.vm.network "forwarded_port", guest: 8080, host: 18080
    #config.vm.network "forwarded_port", guest: 8761, host: 18761
    #config.vm.network "forwarded_port", guest: 8989, host: 18989
    hateoas1.vm.provider "virtualbox" do |v|
      v.memory = 1024
      v.cpus = 1
    end
  #  config.vm.provider "docker" do |d|
    hateoas1.vm.provision "docker" do |d|
      d.build_image "/vagrant/hateoas", args: "-t hateoas"
      #d.build_image "/vagrant/haproxy", args: "-t haproxy"
      d.run "hateoas1", image: "hateoas", args: "-p 9000:8080"
      #d.run "hateoas2", image: "hateoas", args: "-p 9000:8080"
      #d.run "haproxy", image: "haproxy"
    end
  end
  config.vm.define("hateoas2") do |hateoas2|
    hateoas2.vm.box = "centos/7"
    hateoas2.vm.network "private_network", ip: "192.168.15.22"
    hateoas2.vm.provider "virtualbox" do |v|
      v.memory = 1024
      v.cpus = 1
    end
    hateoas2.vm.provision "docker" do |d|
      d.build_image "/vagrant/hateoas", args: "-t hateoas"
      d.run "hateoas2", image: "hateoas", args: "-p 9000:8080"
    end
  end
  config.vm.define("haproxy") do |haproxy|
    haproxy.vm.box = "centos/7"
    haproxy.vm.network "private_network", ip: "192.168.15.20"
    haproxy.vm.provider "virtualbox" do |v|
      v.memory = 1024
      v.cpus = 1
    end
    #haproxy.vm.provision "docker" do |d|
    #  d.build_image "/vagrant/haproxy", args: "-t haproxy-443"
    #  d.run "haproxy-443-piscue", image: "haproxy-443"
    #end
    config.vm.provision "file", source: "haproxy/haproxy.cfg", destination: "/tmp/haproxy.cfg"
    config.vm.provision "shell", privileged: true, inline: <<-SHELL
      mkdir /usr/local/etc/haproxy && \
      mv /tmp/haproxy.cfg /usr/local/etc/haproxy/haproxy.cfg
      yum -y install openssl haproxy >/dev/null 2>&1
    #adduser -D haproxy
      export CERT='/C=ES/ST=Barcelona/L=Barcelona/CN=test@example.com' && \
      openssl genrsa -out ca.key 4096 >/dev/null 2>&1 && \
      openssl req -new -x509 -days 365 -key ca.key -out ca.crt -subj "$CERT" && \
      openssl genrsa -out server.key 1024 >/dev/null 2>&1 && \
      openssl req -new -key server.key -out server.csr -subj "$CERT" && \
      openssl x509 -req -days 365 -in server.csr -CA ca.crt -CAkey ca.key \
      -set_serial 01 -out server.crt && \
      openssl genrsa -out client.key 1024 >/dev/null 2>&1 && \
      openssl req -new -key client.key -out client.csr -subj "$CERT" && \
      openssl x509 -req -days 365 -in client.csr -CA ca.crt -CAkey ca.key \
      -set_serial 02 -out client.crt && \
      cat server.crt server.key > server.pem && \
      mkdir /etc/ssl/private/ && \
      mv server.pem /etc/ssl/private/example.com.pem
      haproxy -f /usr/local/etc/haproxy/haproxy.cfg
    SHELL
  end
end
