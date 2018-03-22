Vagrant.configure("2") do |config|
  config.vm.box = "centos/7"
  config.vm.provider "virtualbox" do |vb|
      vb.gui = false
      vb.memory = ENV["VM_MEMORY"] || "1024"
  end
  config.vm.network "forwarded_port", guest: 8080, host: 9000
  config.vm.provision "shell", privileged: true, inline: <<-SHELL
    function install {
      echo installing $1
      shift
      yum -y install "$@" >/dev/null 2>&1
    }
    yum -y update >/dev/null 2>&1
    install "Git" git
    #install "Java" java-1.8.0-openjdk
    install "Java devel" java-1.8.0-openjdk-devel
    install "Wget" wget
    #install "Maven" maven
    wget -nv http://mirror.olnevhost.net/pub/apache/maven/maven-3/3.1.1/binaries/apache-maven-3.1.1-bin.tar.gz
    tar xf apache-maven-3.1.1-bin.tar.gz
    mv apache-maven-3.1.1 /usr/local/apache-maven
    # go to unprivileged user
  SHELL
  config.vm.provision "shell", privileged: false, inline: <<-SHELL
    echo 'export M2_HOME=/usr/local/apache-maven' >> ~/.bashrc
    echo 'export M2=$M2_HOME/bin' >> ~/.bashrc
    echo 'export PATH=$M2:$PATH' >> ~/.bashrc
    source ~/.bashrc
    git clone https://github.com/spring-projects/spring-boot
    cd spring-boot/spring-boot-samples/spring-boot-sample-hateoas
    mvn spring-boot:run
  SHELL
end
