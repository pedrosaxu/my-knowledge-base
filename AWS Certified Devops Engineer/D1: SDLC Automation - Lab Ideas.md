# SDLC Automation
## Code Commit
#### Repositório e atividades rotineiras de um repositório
- [x] Criar um repositório no CodeCommit
- [x] Clonar o repositório
- [x] Criar um arquivo
- [x] Adicionar os arquivos do curso
- [x] Commitar o arquivo
- [x] Pushar o arquivo
- [x] Criar uma branch
- [x] Criar um arquivo na branch
- [x] Adicionar o arquivo
- [x] Commitar o arquivo
- [x] Pushar o arquivo
- [x] Criar um pull request
- [x] Aprovar o pull request
- [x] Merge do pull request
- [x] Deletar a branch
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
  - [ ] Criar um template de Cloudformation que suba um ALB + ASG na VPC Default
    ```
    AWSTemplateFormatVersion: 2010-09-09
    Description: >
      This template deploys a simple ASG + ALB
    Parameters:
      VpcId:
        Type: AWS::EC2::VPC::Id
        Description: VPC ID
      SubnetId:
        Type: AWS::EC2::Subnet::Id
        Description: Subnet ID
    Resources:
      ALB:
        Type: AWS::ElasticLoadBalancingV2::LoadBalancer
        Properties:
          Name: ALB
          Scheme: internet-facing
          Subnets:
            - !Ref SubnetId
          SecurityGroups:
            - !Ref ALBSG
      ALBListener:
        Type: AWS::ElasticLoadBalancingV2::Listener
        Properties:
          LoadBalancerArn: !Ref ALB
          Port: 80
          Protocol: HTTP
          DefaultActions:
            - Type: forward
              TargetGroupArn: !Ref ALBTargetGroup
      ALBTargetGroup:
        Type: AWS::ElasticLoadBalancingV2::TargetGroup
        Properties:
          Name: ALBTargetGroup
          Port: 80
          Protocol: HTTP
          VpcId: !Ref VpcId
          HealthCheckIntervalSeconds: 30
          HealthCheckPath: /
          HealthCheckProtocol: HTTP
          HealthCheckTimeoutSeconds: 5
          HealthyThresholdCount: 2
          UnhealthyThresholdCount: 2
          Matcher:
            HttpCode: 200
      ALBSG:
        Type: AWS::EC2::SecurityGroup
        Properties:
          GroupDescription: ALBSG
          VpcId: !Ref VpcId
          SecurityGroupIngress:
            - IpProtocol: tcp
              FromPort: 80
              ToPort: 80
                CidrIp: 0.0.0.0/0
          SecurityGroupEgress:
            - IpProtocol: tcp
              FromPort: 80
              ToPort: 80
              CidrIp: 0.0.0.0/0
      ASG:
        Type: AWS::AutoScaling::AutoScalingGroup
        Properties:
          AutoScalingGroupName: ASG
          VPCZoneIdentifier:
            - !Ref SubnetId
          LaunchConfigurationName: !Ref LaunchConfig
          MinSize: 1
          MaxSize: 1
          DesiredCapacity: 1
          TargetGroupARNs:
            - !Ref ALBTargetGroup
      LaunchConfig:
        Type: AWS::AutoScaling::LaunchConfiguration
        Properties:
          LaunchConfigurationName: LaunchConfig
          ImageId: ami-0d5eff06f840b45e9
          InstanceType: t2.micro
          SecurityGroups:
            - !Ref LaunchConfigSG
          UserData:
            Fn::Base64: !Sub |
              #!/bin/bash
              yum update -y
              yum install -y httpd
              systemctl start httpd
              systemctl enable httpd
              echo "Congratulations! You have successfully deployed your application." > /var/www/html/index.html
      LaunchConfigSG:
        Type: AWS::EC2::SecurityGroup
        Properties:
          GroupDescription: LaunchConfigSG
          VpcId: !Ref VpcId
          SecurityGroupIngress:
            - IpProtocol: tcp
              FromPort: 80
              ToPort: 80
              SourceSecurityGroupId: !Ref ALBSG
          SecurityGroupEgress:
            - IpProtocol: tcp
              FromPort: 80
              ToPort: 80
              CidrIp: 0.0.0.0/0
    Outputs:
      ALB:
        Description: ALB
        Value: !Ref ALB
      ALBEndpoint:
        Description: ALBEndpoint
        Value: !GetAtt ALB.DNSName
      ALBListener:
        Description: ALBListener
        Value: !Ref ALBListener
      ALBTargetGroup:
        Description: ALBTargetGroup
        Value: !Ref ALBTargetGroup
      ALBSG:
        Description: ALBSG
        Value: !Ref ALBSG
      ASG:
        Description: ASG
        Value: !Ref ASG
      LaunchConfig:
        Description: LaunchConfig
        Value: !Ref LaunchConfig
      LaunchConfigSG:
        Description: LaunchConfigSG
        Value: !Ref LaunchConfigSG
    StackName: TestStack
    ```

  - [ ] Deploy da aplicação na infra de Testes
  - [ ] Criar um Lambda que busque o output ALBEndpoint da stack de CloudFormation finalizada e faça um GET na URL
    ```
    import requests
    import json
    import boto3

    def lambda_handler(event, context):
        client = boto3.client('cloudformation')
        response = client.describe_stacks(
            StackName='TestStack'
        )
        url = response['Stacks'][0]['Outputs']['ALBEndpoint']['OutputValue']
        r = requests.get(url)
        if r.status_code == 200:
            return {
                'statusCode': 200,
                'body': json.dumps('Congratulations')
            }
        else:
            return {
                'statusCode': 400,
                'body': json.dumps('Something went wrong')
            }
    ```
- [ ] Deletar a infra de Testes
- [ ] Manual approval
- [ ] Deploy to PROD Infra

## Code Build

#### Pipeline padrão com teste
- [ ] Criar um CodeBuild Project
  - [ ] Name: TestForCongratulations
  - [ ] Source: CodeCommit/main/latest
  - [ ] Environment: Managed Image/Ubuntu/Latest
  - [ ] New Service Role
  - [ ] Buildspec: Use a buildspec file
- [ ] Criar um arquivo buildspec.yml
  ```
  version: 0.2
  phases: 
    install: 
      runtime-versions:
        nodejs: 10
      commands:
        - echo "installing dependencies"
    pre_build:
      commands:
        - echo "we are in the pre build phase"
    build:
      commands:
        - echo "we are in the build phase"
        - echo "running unit tests"
        - grep -Fq "Congratulations" index.js
    post_build:
      commands:
        - echo "we are in the post build phase"
        - echo "build completed on `date`"
  ```

---
#### Pipeline interna /VPC com teste de ping 
- [ ] Criar uma EC2
  - [ ] Ubuntu 20.04
  - [ ] SG: *
- [ ] Criar um CodeBuild Project
  - [ ] Name: TestForPing
  - [ ] Source: CodeCommit/main/latest
  - [ ] Environment: Managed Image/Ubuntu/Latest
  - [ ] Select Default VPC
  - [ ] New Service Role
  - [ ] Buildspec: Use a buildspec file
  - [ ] Buildspec Name: buildspec_ping.yml
  - [ ] Variable: ec2_ip = IP Interno da EC2 criada
- [ ] Criar um arquivo buildspec_ping.yml
  ```
  version: 0.2
  phases: 
    install: 
      runtime-versions:
        nodejs: 10
      commands:
        - echo "installing dependencies"
    pre_build:
      commands:
        - echo "we are in the pre build phase"
    build:
      commands:
        - echo "we are in the build phase"
        - echo "running unit tests"
        - ping -c 1 $ec2_ip
    post_build:
      commands:
        - echo "we are in the post build phase"
        - echo "build completed on `date`"
  ```

---
#### Integrando CodeBuild com CodePipeline
- [ ] Criar um Stage no CodePipeline
  - [ ] Stage Name: BuildAndTest
    - [ ] Action Name: TestForCongratulations
    - [ ] Action Provider: AWS CodeBuild
    - [ ] Input Artifact: SourceArtifact
    - [ ] Project Name: TestForCongratulations
    - [ ] Output Artifact: OutputofTestForCongratulations
  
    - [ ] Action Name: TestForPing
    - [ ] Action Provider: AWS CodeBuild
    - [ ] Input Artifact: SourceArtifact
    - [ ] Project Name: TestForPing
    - [ ] Output Artifact: OutputofTestForPing

---
### Extras [Code Build]
#### Configurando uma validação de PR Avançada com CodeCommit e CodeBuild
> https://aws.amazon.com/blogs/devops/validating-aws-codecommit-pull-requests-with-aws-codebuild-and-aws-lambda/
- [ ] Criar 1 função lambda para comentar que o build foi iniciado 
  ```
  import boto3
  import datetime

  def lambda_handler(event, context):
      codecommit = boto3.client('codecommit')
      pr_id = event['detail']['pullRequestId']
      repo_name = event['detail']['repositoryNames'][0]
      comment = f"Build started at {datetime.datetime.now()}"
      codecommit.post_comment_for_pull_request(
          pullRequestId=pr_id,
          repositoryName=repo_name,
          beforeCommitId='',
          afterCommitId='',
          content=comment
      )
  ```
  
- [ ] Criar 1 função lambda para comentar que o build foi finalizado e publicar o Build Badge
  ```
  import boto3

  def lambda_handler(event, context):
      codecommit = boto3.client('codecommit')
      codebuild = boto3.client('codebuild')
      build_id = event['detail']['build-id']
      build_status = event['detail']['build-status']
      pr_id = codebuild.batch_get_builds(ids=[build_id])['builds'][0]['source']['location'].split('/')[-1]
      repo_name = codebuild.batch_get_builds(ids=[build_id])['builds'][0]['source']['location'].split('/')[-2]
      badge_url = codebuild.batch_get_builds(ids=[build_id])['builds'][0]['artifacts']['location']
      comment = f"Build {build_status}! [![Build Status]({badge_url})](https://console.aws.amazon.com/codesuite/codebuild/projects/{repo_name}/history?region=us-east-1)"
      codecommit.post_comment_for_pull_request(
          pullRequestId=pr_id,
          repositoryName=repo_name,
          beforeCommitId='',
          afterCommitId='',
          content=comment
      )
  ```
- [ ] Configurar triggers no EventBridge para disparar os Lambdasde Build iniciado quando um PR for criado/alterado no CodeCommit e Build Status quando um build for finalizado no CodeBuild
  - [ ] Build iniciado
    - [ ] EventBridge
      - [ ] Create Rule
        - [ ] Name: CodeCommit-PR-Create-Update
        - [ ] Event Pattern
          ```
          {
            "source": [
              "aws.codecommit"
            ],
            "detail-type": [
              "CodeCommit Pull Request State Change"
            ],
            "detail": {
              "event": [
                "pullRequestCreated",
                "pullRequestSourceBranchUpdated",
                "pullRequestStatusUpdated"
              ]
            }
          }
          ```
        - [ ] Targets
          - [ ] Lambda Function: CodeCommit-PR-Create-Update
             
    - [ ] Create Rule
      - [ ] Name: CodeBuild-Build-Status
      - [ ] Event Pattern
        ```
        {
          "source": [
            "aws.codebuild"
          ],
          "detail-type": [
            "CodeBuild Build State Change"
          ],
          "detail": {
            "build-status": [
              "SUCCEEDED",
              "FAILED",
              "STOPPED"
            ]
          }
        }
        ```
      - [ ] Targets
        - [ ] Lambda Fucntion: CodeBuild-Build-Status

---
#### Criar um BuildProject para passar o Checkov nos arquivos e publicar um Test Report com o resultado
- [ ] Criar um BuildProject
  - [ ] Name: CheckovTest
  - [ ] Source: CodeCommit/main/latest
  - [ ] Environment: Managed Image/Ubuntu/Latest
  - [ ] New Service Role
  - [ ] Buildspec: Use a buildspec file
  - [ ] Buildspec Name: buildspec_checkov.yml
  - [ ] Variable: ec2_ip = IP Interno da EC2 criada
- [ ] Criar um arquivo buildspec_checkov.yml
  ```
  version: 0.2
  phases: 
    install: 
      runtime-versions:
        python: 3.8
      commands:
        - echo "installing dependencies"
        - pip install checkov
    pre_build:
      commands:
        - echo "we are in the pre build phase"
    build:
      commands:
        - echo "we are in the build phase"
        - echo "running unit tests"
        - checkov -d .
        - checkov -d . -o junitxml > checkov_report.xml
    post_build:
      commands:
        - echo "we are in the post build phase"
        - echo "build completed on `date`"
    reports:
      checkov_report:
        files:
          - '**/*_report.xml'
        file-format: JunitXml
  ```
