
### TO-DO
- [x] 27/07: Kick-off com o cliente:
	- [x] Detalhamento de tipos de aplicações
	- [x] Detalhamento de tipos de ambientes de deployment
	- [x] <font color="red">Detalhamento de serviços da Dedalus/Algar:</font> Ederson / HS
	- [x] Criação de novos ambientes pela Algar
		- [x] Azure Devops
	- [x] <font color="red">Recepção de usuários e nomes dos pipelines</font>
		- [marco.basito@hsprevent.com.br](mailto:marco.basito@hsprevent.com.br)
		- [roger.ikeda@hsprevent.com.br](mailto:roger.ikeda@hsprevent.com.br)
		- [marcio.garcia@hsprevent.com.br](mailto:marcio.garcia@hsprevent.com.br)
		- [arthur.nascimento@hsprevent.com.br](mailto:arthur.nascimento@hsprevent.com.br)
		- [leandro.bizerra@hsprevent.com.br](mailto:leandro.bizerra@hsprevent.com.br)
		- [rafael.cardoso@hsprevent.com.br](mailto:rafael.cardoso@hsprevent.com.br)
		- [lucas.pinheiro@hsprevent.com.br](mailto:lucas.pinheiro@hsprevent.com.br)
		- [ederson.santos@hsprevent.com.br](mailto:ederson.santos@hsprevent.com.br)
		- [philipe.silva@hsprevent.com.br](mailto:philipe.silva@hsprevent.com.br)
		- [andre.silva@hsprevent.com.br](mailto:andre.silva@hsprevent.com.br)
		- [igor.silva@hsprevent.com.br](mailto:igor.silva@hsprevent.com.br)
	- [x] Criação de usuários no Azure Devops
	- [x] <font color="red">Recepção de nomes dos pipelines</font>
	- [x] <font color="red">Detalhamento do processo de build pela HS</font>
	- [x] Desenvolvimento do processo de build das aplicações
		- [x] **hsprevent-web-vue-identifier_control** -> npm 8.5.0
		```
		sudo npm install -g yarn
		yarn && yarn build --prod
		```

		- [x] **hsprevent-web-angular-identifier_admin** -> npm 8.5.0
		```
		sudo npm install -g yarn
		yarn && npm run publish
		```

		- [x] **hsprevent-web-react-eva_operation** -> npm 8.5.0
		```
		npm i && npm run build
		```

		- [x] **hsprevent-api-java-eva_auth** -> maven 3.8.6
		```
		mvn package
		```

		- [x] **hsprevent-api-dotnet-identifier_admin** -> nuget v?
		```
		
		```

		- [x] **hsprevent-api-dotnet-identifier_control_database** -> nuget v?
		```
		
		```

		- [x] **hsprevent-api-golang-identifier_control** -> go v?
		```
		
		```


- [ ] Desenvolvimento do processo de build e push das imagens
	- [ ] <font color = "red"> Criação de infraestrutura nos ambientes existentes</font>
		- [ ] Registries AWS e OCI -> Até dia 11/08
		- [ ] Cluster Kubernetes AWS e OCI -> Até dia 22/08
	- [ ] Acessos aos ambientes existentes pela HS:
		- [ ] OCI
		- [ ] AWS
	- [ ] Configuração de CI
		- [ ] Gitflow
	- [ ] Detalhamento do processo de deploy pela HS
	- [ ] Configuração do ambiente de deploy pela Dedalus
		- [ ] Criação do usuário sistêmico para o Azure Devops
		- [ ] Instalação do Azure Devops
		- [ ] Revisão de permissão dos diretórios de destino do deploy
	- [ ] Demonstração de piloto de deploy pela Dedalus
  
---

### DÚVIDAS
- [x] **Nós realizaremos todas as configurações nas máquinas de destino?** 
	Sim, eles proverão os acessos pra gente.
- [x] **Configuração de rede (Azure Devops > Redes de deploy) vão ficar com o cliente?** 
	Sim, AWS é nosso time de operação (se for as contas que estão conosco), OCI é com eles.
- [x] **Cliente já possui conta na Azure?** 
	Se não tiver, a Algar quem realiza a criação.
- [x] **Quais produtos serão utilizados?** 
	Azure Pipelines.
- [x] **10 aplicações, quantas de quais tipos? 
	- Márcio Luis:
		- 2 principais tech stacks:
			- React -> Nova, ainda não existe
			- Java -> Nova, ainda não existe
		- Demais stacks:
			- Vue
			- Go
		- Diferem entre Backend e Frontend, será definido quantos de cada ao longo do projeto.
- [x] **Tipo de deploy: Em vm, container, kubernetes?** 
	!!! Verificar com o cliente.
	Varia de acordo com a aplicação - quase todos serão VM, somente 2 aplicações Java querem avaliar se será em Serverless ou EC2
	- Márcio Luis:
		- 100% Kubernetes no futuro.
	- Arthur Nascimento:
		- À princípio será em VM
		- Inicialmente não contempla Kubernetes, porém ainda não comportam
- [x] **Onde ficarão os artefatos? Artefatos do Pipeline ou algum repositório externo? (Nexus?)**
	- Márcio Luis:
		- Ficará no Azure Devops mesmo.
- [x] **Acesso ao GitLab:**
	- Márcio Luis
		- Não será liberado
		- Criaremos novos repositórios no Azure Repos
		- No futuro, os demais serão migrados para o Azure Repos
- [x] **Como é o processo de desenvolvimento?**
	- [ ] Utilizam o Gitflow hoje
	- [ ] Vão mudar.
- [x] **Como desejam que seja o processo de deploy? 100% automatizado?**
	- [ ] Aprovação direto no pull-request
- [ ] **Precisaremos realizar alguma configuração na AWS e OCI (destino do deploy) - configurações de redes e etc, ou só receberemos o acesso e configuraremos as máquinas para prepará-las para deploy?**
	   !!! Verificar com a Dedalus
- [ ] **Cliente já possui NS contratado?**
      !!! Verificar com a Dedalus

---
### DADOS DO PROJETO
- **Proposta**: [DP.35908.22](https://dedalusprime.lightning.force.com/lightning/r/Opportunity/0063r00001HVxuQAAT/view)

- **Arquitetura:** ![[Pasted image 20220727095939.png]]

- **Pontos focais:**
	- **Arthur Nascimento**: Coordenador de Devops
	- **Leandro Bizerra**: Engenheiro de Devops

- **Detalhes das Aplicações:**
|**Status**| **Tipo**    | **Nome da Aplicação**                      | **Linguagem** | **Gerenciador de pacotes**     | **Infraestrutura**       | **Nuvem**     |
|------|---------|----------------------------------------|-----------|----------------------------|----------------------|-----------|
|IMAGE-OK|frontend | hsprevent-web-vue-identifier_control   | Vue       | NPM                        | Docker ou Kubernetes | OCI       |
|IMAGE-OK|frontend | hsprevent-web-angular-identifier_admin | Angular   | NPM                        | Docker ou Kubernetes | AWS       | 
|IMAGE-OK|frontend | hsprevent-web-react-eva_operation      | React     | NPM                        | Docker ou Kubernetes | OCI       |
|IMAGE-OK|backend  | hsprevent-api-java-eva_auth            | Java      | Maven                      | Docker ou Kubernetes | OCI       |
|IMAGE-OK|backend  | hsprevent-api-dotnet-identifier_admin  | DotNet    | Nuget                    | Docker ou Kubernetes | AWS       |
|IMAGE-OK|backend  | hsprevent-api-dotnet-identifier_control_database  | DotNet    | Nuget                      | Docker ou Kubernetes | OCI       |
|IMAGE-OK|backend  | hsprevent-api-golang-identifier_control| Golang    | Go                         | Docker ou Kubernetes | OCI       |

---

### DOCUMENTAÇÃO
- 

---

### DADOS / SECRETS
- 
---

