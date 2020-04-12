# 앤서블 노드 추가하기

이전 절에서 앤서블 서버를 설치하였으므로, 이번 장에서는 앤서블 노드를 설치하여 테스트 환경을 구축하는 것을 목표로 한다.

다음과 같이 Vagrantfile 에 3개의 앤서블 노드를 추가할 수 있도록 정의한다.  
기존에 Vagrantfile에서 앤서블 서버 설정은 두고 4번 라인에 해당하는 Vagrant.configure 정의를 최상단으로 올려서 작성한다.

각 앤서블 노드의 프로비저닝 설정은 앤서블 서버와 크게 다르지 않다.

{% code title="Vagrantfile" %}
```ruby
# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

  # Ansible-Node1
  config.vm.define "ansible-node1" do |cfg|
    cfg.vm.box = "centos/7"
    cfg.vm.provider "virtualbox" do |vb|
      vb.name = "Ansible-Node01"
    end
    cfg.vm.host_name = "ansible-node01"
    cfg.vm.network "public_network", ip: "192.168.0.11"
    cfg.vm.network "forwarded_port", guest: 22, host: 60011, auto_correct: true, id: "ssh"
    cfg.vm.synced_folder "../data", "/vagrant", disabled: true
  end

  # Ansible-Node2
  config.vm.define "ansible-node2" do |cfg|
    cfg.vm.box = "centos/7"
    cfg.vm.provider "virtualbox" do |vb|
      vb.name = "Ansible-Node02"
    end
    cfg.vm.host_name = "ansible-node02"
    cfg.vm.network "public_network", ip: "192.168.0.12"
    cfg.vm.network "forwarded_port", guest: 22, host: 60012, auto_correct: true, id: "ssh"
    cfg.vm.synced_folder "../data", "/vagrant", disabled: true
  end

  # Ansible-Node3
  config.vm.define "ansible-node3" do |cfg|
    cfg.vm.box = "centos/7"
    cfg.vm.provider "virtualbox" do |vb|
      vb.name = "Ansible-Node03"
    end
    cfg.vm.host_name = "ansible-node03"
    cfg.vm.network "public_network", ip: "192.168.0.13"
    cfg.vm.network "forwarded_port", guest: 22, host: 60013, auto_correct: true, id: "ssh"
    cfg.vm.synced_folder "../data", "/vagrant", disabled: true
  end

  # Ansible-Server
  config.vm.define "ansible-server" do |cfg|
    cfg.vm.box = "centos/7"
    cfg.vm.provider "virtualbox" do |vb|
      vb.name = "Ansible-Server"
    end
    cfg.vm.host_name = "ansible-server"
    cfg.vm.network "public_network", ip: "192.168.0.10"
    cfg.vm.network "forwarded_port", guest: 22, host: 60010, auto_correct: true, id: "ssh"
    cfg.vm.synced_folder "../data", "/vagrant", disabled: true
    cfg.vm.provision "shell", inline: "yum install ansible -y"
    cfg.vm.provision "file", source: "ansible_env_ready.yml", destination: "ansible_env_ready.yml"
    cfg.vm.provision "shell", inline: "ansible-playbook ansible_env_ready.yml"
  end
end
```
{% endcode %}

위와 같이 Vagrantfile 을 작성하였으면, 이를 적용하기 위해 기존에 실행되고 있던 앤서블 서버를 강제 옵션을 주어 삭제한다. 삭제 명령어는 아래와 같다.

```ruby
vagrant destroy -f
```

Vagrantfile에 적용한 후, 삭제 명령어를 실행했기 때문에, 아래 출력된 ansible-node3, ansible-node2 에 대한 로그는 무시해도 된다.

![](.gitbook/assets/image%20%288%29.png)

이후 앤서블 서버 1대와 앤서블 노드 3대의 가상 머신을 실행하기 위해 다시 아래 명령어를 실행한다.  
\(프로비저닝에 오래 걸리니 인내심을 갖고 기다려야 한다...\)

```ruby
vagrant up
```

![](.gitbook/assets/image%20%285%29.png)

가상 머신이 모두 설치 되면 아래와 같이 동작 중인 것을 확인할 수 있으며, vagrant status 명령어를 통해서도 확인할 수 있다.

![](.gitbook/assets/image%20%284%29.png)



