---
description: 앤서블과 베이그런트에 대해 알아보고 가상 머신(VM) 을 실행해보도록 하자.
---

# 시작하기

## 앤서블 \(Ansible\)

### 개요

앤서블은 서버, 저장소, 네트워크, 운영체제 등을 효율적으로 관리할 수 있는 도구를 의미한다.  


### 앤서블의 특징

이와 같이, IT 인프라를 관리 도구해주는 툴로는 Puppet, Chef, Salt 등이 존재한다.  
다른 도구들과 비교했을 때, 앤서블의 가장 큰 특징은 에이전트 설치가 필요 없다는 것이다.

  
이말은 즉, 앤서블은 에이전트 설치 부재로 인한 구성 관리의 복잡도를 낮추고 보안 이슈의 가능성을 낮춘다는 것을 의미한다. 또한, 멱등성이 보장되어 연산이 중첩되어 적용될일이 없으므로 실수를 방지해주고 러닝 커브가 낮아 배우기 쉽다는 특징을 갖고 있다.

### 앤서블의 주요 개념

앤서블은  IT 인프라의 관리를 자동화 하는 것을 목표로 한다.  
이에 자동화를 위한 앤서블에서 설명하는 아래의 표로 정리된 몇가지 개념이 필요하다.

| 이름 | 설명 |
| :--- | :--- |
| **인벤터리 \(Inventory\)** | 엔서블이 관리하고자 하는 서버들을 정의해놓은 텍스트 파일이다. |
| **플레이북 \(PlayBook\)** | 엔서블이 관리하는 서버에서 구성 또는 실행되는 일련의 작업들을 YAML 파일 형태로 정의한 스크립트 파일이다. 다른말로, Infrastrure as a code 라고 이해하면 쉬울 듯..?  |
| **모듈 \(Module\)** | 앤서블이 관리하는 서버에서 사용하는 일종의 API 이며, ansible 또는 ansible-playbook 을 활용한 스크립트 형태로 실행이 가능하다. |

위 3가지 개념은 어 장비에서 어떤 명령을 내려 서버를 구성할 것인지에 대한 앤서블에서 사용하는 정의를 나타낸 것이라고 보면 될것이다.

먼저 앤서블을 작업하기 위해선 아래와 같이 사전에 구성해야 할 것들이 있다.

1. 가상 머신 관리 도구를 설치한다. \(VirtualBox, vmware ...\)
2. 서버 이미지를 다운로드 받는다. \(Ubuntu, CentOS ...\)
3. 가상 머신 관리 도구 위자드를 실행해서 가상 머신을 생성한다.
4. 앤서블 실습을 위해, 네트워크 브릿지 설정을 통해 IP를 수동으로 잡아준다.
5. 1~4번 작업을 원하는 작업 서버 노드 개수\(N\) 만큼 반복한다.
6. 앤서블을 실행하고자 하는 관리 주체 서버에 ansible 패키지를 설치한다. \( yum install ansible -y \)

위의 구성을 완료하면 대략 아래의 모양이 된다.

![](.gitbook/assets/image%20%282%29.png)

위의 작업이 복잡하진 않지만, 시간만 많이 잡아먹는 일종의 노가다 작업이 될 수 있을 것이다. 또한, 위의 작업을 진행하다가 인프라 구성 도중에 실수할 확률이 높아 설정이 잘못되어 엉뚱한 곳에 시간을 허비할 수도 있다.  
이를 조금 더 쉽게 인프라 구성을 할 수 있도록 도와주는 **베이그런트\(Vagrant\)** 라는 가상 환경 시스템 구성 도구가 있다.

## 베이그런트 \(Vagrant\)

![](.gitbook/assets/1_cok405whjcrx-0rcbetika.png)

### 개요

베이그런트란 시스템의 프로비저닝 도구로 사용자의 요구에 맞추어 시스템 자원 배치, 할당 배포해 두었다가 필요할 때 사용할 수 있는 상태로 만들어주는 도구이다.

### 설치하기

{% hint style="info" %}
베이그런트 사용을 진행하기 위해서는 사전에 가상머신 관리 도구가이 설치되어 있어야 한다.   
여기서는 \(VirtualBox 6.0 버전을 사용한다.\)
{% endhint %}



다음 베이그런트 홈페이지\([https://www.vagrantup.com/](https://www.vagrantup.com/)\) 에서 각 플랫폼 OS에 맞는 설치 파일을 다운로드 받아 설치한다.

### 가상 머신 프로비저닝 하기

가상 머신을 생성하기 위해서는 먼저 베이그런트 초기화 명령을 실행해야 한다.

```text
vagrant init
```

| 명령어 | 설명 |
| :--- | :--- |
| vagrant init | 프로비저닝을 위한 스크립트 생성 \(Vagrantfile\) |
| vagrant up | 베이그런트 파일을 읽어 프로비저닝 진행 |
| vagrant halt | 베이그런트에서 다루는 가상 머신 삭제 |
| vagrant destroy | 베이그런트에서 관리하는 가상 머신 삭제 |
| vagrant ssh | 베이그런트에서 관리하는 가상 머신에 ssh로 접속 |
| vagrant provision | 베이그런트에서 관맇하는 가상 머신에 변경된 설정을 적용 |

vagrant init 명령을 실행한 디렉터리에 Vagrantfile이 있는 것을 확인할 수 있다.  
vagrant up 명령어를 통해 그냥 사용하게 되면, base 라는 가상 머신을 찾을 수 없으므로 에러가 발생한다.  
해당 파일을 열어 config.vm.box = "base" 부분을 변경해야 한다.

해당 설정 값\(config.vm.box\)은 가상 머신에 대한 이름을 의미하는데, 이에 설치할 수 있는 가상 머신은   
다음 페이지\([https://app.vagrantup.com/boxes/search](https://app.vagrantup.com/boxes/search)\) 에 접속하여 확인할 수 있다.  


![Vagrant Cloud\(https://app.vagrantup.com/boxes/search\)](.gitbook/assets/image%20%286%29.png)

여기서는 centos 를 설치해보도록 한다.  
먼저 Vagrantfile에 config.vm.box = "centos/7" 을 입력하도록 한다.

{% code title="Vagrantfile" %}
```ruby
# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "centos/7"
end

```
{% endcode %}

  
이제 아래 명령어를 통해 centos/7 에 해당하는 가상 머신을 다운로드를 받도록 한다.  
\(VM 생성 명령어를 입력하면 존재하지 않는 box는 내부적으로 다운로드가 진행된다\)

{% hint style="danger" %}
베이그런트 실습 진행 시, VirtualBox는  현재 VirtualBox &lt;= 6.0 까지만 지원하도록 되어있으니, 유의하도록 한다.
{% endhint %}

```text
vagrant up --provider=virtualbox
```

![](.gitbook/assets/image%20%287%29.png)

가상머신 설치가 완료되면 VirtualBox 를 실행하여 확인해본다.  
베이그런트가 만들어낸 centos 가상 머신이 존재하는 것을 확인할 수 있다.  
이때, 가상머신의 이름은 별다른 이름을 지정하지 않았으므로, "**HashiCorp\_default\_1577376330096\_42496**" 와 같이 생성된다.

이후, 아래 명령어를 통해 해당 가상머신에 접속한다.

```bash
# 해당 가상 머신에 접속한다.
vagrant ssh

# 접속 종료 후, 게스트 에디션을 설치한다.
vagrant plugin install vagrant-vbguest

# 가상 머신을 삭제한다.
vagrant destroy

# 가상 머신을 생성한다.
vagrant up
```

다음과 같이 가상 머신을 생성하게 되면, 자동으로 가상 머신을 생성하고 해당 가상 머신에 게스트 에디션까지 설치를 완료한다.





