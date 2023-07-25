# SDLC Automation
## Code Commit
#### Repositório e atividades rotineiras de um repositório
- [ ] Criar um repositório no CodeCommit
- [ ] Clonar o repositório
- [ ] Criar um arquivo
- [ ] Adicionar os arquivos do curso
- [ ] Commitar o arquivo
- [ ] Pushar o arquivo
- [ ] Criar uma branch
- [ ] Criar um arquivo na branch
- [ ] Adicionar o arquivo
- [ ] Commitar o arquivo
- [ ] Pushar o arquivo
- [ ] Criar um pull request
- [ ] Aprovar o pull request
- [ ] Merge do pull request
- [ ] Deletar a branch
- [ ] Desfazer a última branch
- [ ] Deletar o repositório

## Code Pipeline
#### Pipeline simples com 2 ambientes e aprovaçõa manual
- [ ] Criar dois ambientes básicos no Beanstalk (Sample + Simple Instance)
  - [ ] Dev
  - [ ] Prod
- [ ] Criar um pipeline
  - [ ] Source is the previuously created CodeCommit repository
  - [ ] No build
  - [ ] Deploy to Beanstalk (Dev)
  - [ ] Add manual approval
  - [ ] Deploy to Beanstalk (Prod)

### Extras [Code Pipeline]
#### Pipeline avançado com Cloudformation e Testes em infra intermediária
- [ ] Criar um novo repositório
- [ ] Subir o código da aplicação para o repositório
- [ ] Testar a aplicação em ma infra intermediária
  - [ ] Criar um template de Cloudformation que suba um ALB + ASG
  - [ ] Deploy da aplicação na infra de Testes
  - [ ] Teste HTTP na infra de Testes
  - [ ] Deletar a infra de Testes
- [ ] Manual approval
- [ ] Deploy to PROD Infra

