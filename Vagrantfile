Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/trusty64"
  config.vm.provider "virtualbox" do |v|
    v.memory = 3072
  end
  config.vm.define "ubuntu-minikube" do |um|
  	um.vm.hostname = "ubuntu-minikube"
    um.vm.network "private_network", ip: "10.100.199.200"
    um.vm.provision :shell, path: "bootstrap.sh"
    um.vm.provision :shell, inline: "PYTHONUNBUFFERED=1 ansible-playbook /vagrant/ansible/ubuntu-minikube.yml -c local"
    um.vm.provision :file,  source: "./kubectl/v1.9.2/kubectl", destination: "/home/vagrant/kubectl"
    um.vm.provision :shell, inline: "chmod +x /home/vagrant/kubectl"
    um.vm.provision :shell, inline: "sudo mv /home/vagrant/kubectl /usr/local/bin/"
    um.vm.provision :file,  source: "./minikube/v0.25.0/minikube-linux-amd64", destination: "/home/vagrant/minikube"
    um.vm.provision :shell, inline: "chmod +x /home/vagrant/minikube"
    um.vm.provision :shell, inline: "sudo mv /home/vagrant/minikube /usr/local/bin/"
    um.vm.provision :shell, inline: "minikube start --vm-driver=none", run: "always"
  end
  if Vagrant.has_plugin?("vagrant-cachier")
    config.cache.scope = :box
  end
end