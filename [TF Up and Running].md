
Fala pessoal, tudo bom?

Surpreendendo a todos inclusive a mim mesma com esse segundo vídeo, estamos aqui em mais um vídeo do canal!

Esse vai ser o primeiro vídeo da nossa série sobre Terraform!!

Meu objetivo é fazer desta uma série inteiramente prática, sempre com o terminal e o código aberto para não só falar como também demonstrar o que cada coisa faz de fato!

Infelizmente muitos livros técnicos excelentes são escritos somente na língua inglesa e este também é o caso do livro que será o nosso fio condutor aqui em nossa primeira série, que se chama Terraform Up and Running.

Este é um livro excelente, que traz diversos comparativos com as principais ferramentas utilizadas recentemente para fazer Infraestrutura e Configuração como Código.

**CAP1 - PORQUE TERRAFORM?**
- **Vídeo 1: Devops, IAC e Automação de Antigamente**
	**1. O boom do Devops**
	- Primeiro Manifesto Ágil, o segundo foi o Devops.
	- Cultura Devops > Disciplinas atuais
	- Entregar mais código de forma mais consistente e segura através de uma maior integração entre os times.
	- Automação de tarefas > **Infraestrutura como Código**.
	- Crie, atualize, implemente e destrua recursos de infraestrutura através do código. 

	**2. Categorias da Infraestrutura como código**
	- Atualmente podemos separar a Infraestrutura como Código em 5 principais categorias
		- Scripts sob demanda -> Bash, powershell
		- Configuração como Código -> Ansible
		- Ferramentas de padronização de servidores -> Docker, Packer, Vagrant
		- Ferramentas de orquestração -> Kubernetes
		- Ferramentas de provisionamento -> Terraform, Pulumi...

	**2.1. Scripts sob demanda**
	- São scripts simples que executamos conforme há alguma necessidade, para facilitar uma tarefa repetitiva:
		[ PRÁTICA ]
			- Entregar 5 Servidores Web com o Apache instalado para um time solicitante. Você pode fazer isto na mão, ou simplesmente elaborar um pequeno script:
				- Linux -> Bash
			- Linha de comando 4ever
			- Como escalar?
			- E no Windows?
				- Windows -> Powershel
			- Executar 2 meses depois...
				- Como garantir que a versão do sistema não foi alterada? Ou que a versão do apache não foi atualizada por algum outro usuário? 
			- Condicionais > Complexidade > Conhecimento > Manutenção
- **Vídeo 2: Configuração como Código com Ansible**
	**2.2. Configuração como Código**
		Simplificar a automação de tarefas em diferentes sistemas
			- Declarativas > menos Condicionais
			- Multi-sistemas
			- Chef, Puppet, Salstask e Ansible
			- 
