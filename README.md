# SetupHyper-VNestedVirtualizationonAzure

안녕하세요,,
오래 간만에 인사드립니다.
제가 최근에 Hyper-V 가상화 기술을 다시 공부하게 되었습니다.
Hyper-V 실습을 하기 위해서는 CPU 및 메모리가 충분한 Physical Machine이 필요한데, 사실 이러한 장비를 대부분 상시적으로 가지고 있지는 않습니다.
그런데, Azure를 포함한 Public 클라우드 벤더들이 Hyper-V와 같은 가상화 플랫폼을 IaaS VM으로 지원합니다. 이에 제가 최근에 Azure IaaS VM 에 Hyper-V를 설치해 보았습니다.
본 포스팅에서는 아래와 같은 주제를 다루어 보았습니다.

①	Azure IaaS VM 내에 Hyper-V 기능 설치 및 구성

②	“Hyper-V 호스트 (Azure IaaS VM)” 와 “Hyper-V Guest VM” 사이의 네트워크 통신 구성

③	“Azure 가상 네트워크 내의 VM” 과 “Hyper-V Guest VM” 사이의 네트워크 통신 구성

④	“Hyper-V 호스트 (Azure IaaS VM)” 상의 “Hyper-V Guest VM” 이 인터넷(외부) 통신 구성


위 3가지 주제를 구성하기 위한 기본적인 데모 환경 및 데모 주제에 대한 설명은 아래와 같습니다.

본 문서에서 사용할 데모 환경은 아래와 같습니다.

![image](https://github.com/dongclee/SetupHyper-VNestedVirtualizationonAzure/assets/42400574/faa98f32-bcaf-4417-98af-60fcb9b3ba15)

위 데모 환경의 VM 들의 기본적인 구성 정보입니다.

<img width="697" alt="image" src="https://github.com/dongclee/SetupHyper-VNestedVirtualizationonAzure/assets/42400574/69c22758-d56e-49df-b049-cacfa303584b">


VM 종류	VM 이름	VM IP	VM 서브넷
Azure	HV01	10.0.1.4	NAT (Azure)
		10.0.2.4	LAN (Azure)
		192.168.0.1	InternalNATSwitch (Hyper-V)
Azure	jumpbox	10.0.2.5	LAN (Azure)
Hyper-V	NVVM01	192.168.0.2	InternalNATSwitch (Hyper-V)

HV01 Hyper-V 호스트에 “Routing and Remote Access” 역할을 설치한 후, “NAT” 및 “LAN Routing”기능을 활성화합니다. “NAT” 기능을 활성화한 후, HV01 에서 생성한 Hyper-V VM 들이 “인터넷” 및 “Azure 가상 네트워크 (nvhyperv-vnet , 10.0.0.0/16)” 로의 통신이 가능하기 위해서는 “Routing and Remote Access” 내에서 “정적 경로”를 구성해야 합니다. 아래 테이블을 참조하여 “Routing and Remote Access” 내에서 “정적 경로”를 구성합니다.

<img width="696" alt="image" src="https://github.com/dongclee/SetupHyper-VNestedVirtualizationonAzure/assets/42400574/9e23e48b-8521-4316-a5fd-7bfde772e2c8">

본 문서에서 추후 “Routing and Remote Access” 내에서 “정적 경로”를 구성할 예정입니다.

HV01 Azure VM 및 jumpbox Azure VM이 생성될 Azure 가상 네트워크 및 서브넷 정보는 아래와 같습니다.

	가상 네트워크 이름: nvhyperv-vnet

	가상 네트워크 주소 공간: 10.0.0.0/16

	서브넷 이름: NAT (how the host VM provides connectivity for hosted VMs)

	서브넷 주소 공간: 10.0.1.0/24

	서브넷 이르: LAN (how Azure VMs talk to the host and nested VMs)

	서브넷 주소 공간: 10.0.2.0/24


Azure 가상 네트워크 및 서브넷 상의 VM 들이 HV01 내의 Hyper-V VM 으로의 네트워크 통신이 가능하기 위해서는, Azure 가상 네트워크 및 서브넷 상에 “Azure User Defined Route” 를 생성해야 합니다. 아래 정보를 기반으로 “Azure User Defined Route”를 추후 생성할 예정입니다.

<img width="777" alt="image" src="https://github.com/dongclee/SetupHyper-VNestedVirtualizationonAzure/assets/42400574/daa70c94-9b81-4da0-8807-6319355cdc5e">

이상과 같이 본 포스팅의 내용 설명을 마칩니다.

추후, Azure 상에서 Hyper-V 호스트의 High Availability 를 구성하는 내용을 소개하고자 합니다.

기대해 주세요 ^-^



[20230603 How to Setup Hyper-V Nested Virtualization on Azure.pdf](https://github.com/dongclee/SetupHyper-VNestedVirtualizationonAzure/files/11645085/20230603.How.to.Setup.Hyper-V.Nested.Virtualization.on.Azure.pdf)
