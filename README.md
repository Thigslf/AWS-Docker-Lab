
  RELATÓRIO DE EXECUÇÃO DO LABORATÓRIO AWS DOCKER

 -> Resumo Executivo

Este relatório documenta a execução e a conclusão do laboratório prático de conteinerização de uma aplicação em três camadas (3-tier) implantada em uma instância virtual AWS EC2 Ubuntu.

O objetivo principal do laboratório é disponibilizar uma interface web via Nginx (Frontend), integrada a uma API Node.js (Backend) que consulta e persiste dados em um banco PostgreSQL — foi alcançado com sucesso.


 -> Histórico de Atividades Realizadas 

  -> Realizado o acesso no PowerShell 
  
    . Entrei na pasta Downloads para localizar a chave privada lab.pem 
    . Listei o arquivo lab.pem para confirmar se a chave SSH estava na pasta 
    . Iniciei a conexão com a SSH com o servidor Linux usando a chave la.pem

   <img width="886" height="338" alt="image" src="https://github.com/user-attachments/assets/042da0aa-6f6c-479f-a562-a0fb61c4b67c" />
   <img width="886" height="958" alt="image" src="https://github.com/user-attachments/assets/35e7e778-7a41-41cd-8e31-704825951406" />

  -> Realizado a instalação do Docker
    
    . Entrei no diretório aws-docker, onde o Terraform pré instalou os arquivos base do treinamento
    . Atualizei a lista de pacotes de programas do ubuntu
    . Instalei o Docker (docker.io) e a extensão de orquestração de contêineres (docker-compose-v2)

<img width="836" height="80" alt="image" src="https://github.com/user-attachments/assets/d03ab410-49f5-4d90-8cc6-7764bcdbb76c" />
<img width="886" height="44" alt="image" src="https://github.com/user-attachments/assets/1f13ee17-df17-4f04-907c-18f4bbafdadc" />


    . Liguei o Docker no sistema
    . Adicionei o usuário ubuntu no grupo de usuários com permissão do Docker
    . Recarreguei as permissões na sessão atual

<img width="886" height="89" alt="image" src="https://github.com/user-attachments/assets/5e4d2b5a-5275-407b-80aa-769241e4ad9d" />



-> Realizado a criação do Dockerfile da aplicação Backend (Node.js)
      
      . Criei um novo arquivo no caminho indicado com todo o conteúdo escrito até encontrar a palavra EOF
      . Baixei a imagem base oficial do Node.js na versão alpine 
      . Defini a pasta de trabalho dentro do contêiner onde o código vai rodar
      . Copiei os arquivos de dependências do Node do computador para o contêiner
      . Instalei as bibliotecas de código que o backend precisa para funcionar
      . Copiei o restante dos arquivos da pasta backend para dentro do contêiner
      . Documentei que a aplicação escuta a porta de rede 3000
      . Instrui o Docker a executar a API chamando o comando node server.js assim que o contêiner ligar
<img width="886" height="225" alt="image" src="https://github.com/user-attachments/assets/da1d4977-b33e-4314-844f-102bf0542cb5" />

  

-> Realizado a criação do Dockerfile da aplicação Frontend (Nginx)

    . Utilizei como base o servidor web Nginx
    . Deletei a página de teste padrão original do Nginx
    . Copiei o arquivo de configuração que irá encaminhas as requisições /api/ para a API Node.js
    . Copiei a interface visual web (HTML) para a pasta de exibição do servidor Nginx
    . Liberei a porta padrão de navegação de páginas web HTTP - Porta 80
    . Mantive o servidor Nginx rodando ativamente no primeiro plano do contêiner

<img width="886" height="218" alt="image" src="https://github.com/user-attachments/assets/12516c04-8dfd-4587-8288-a0096ec3a4eb" />


-> Realizado a criação do arquivo de Orquestração 
    
    . Agrupei os 3 componentes da arquitetura
    . Configurei o PostrSQL, passei o script init.sql para popular o banco na inicialização e adicionei um healthcheck (pg_isready) para testar periodicamente se o banco aceita conexões
    . Docker criou a imagem usando o diretório ./backend, passei as credenciais para se comunicar com o serviço db e usei o depends_on: condition: service_healthy para garantir que a API só ligue após o banco estar pronto
    . Compilei o site na pasta ./frontend, mapiei a porta da máquina (80) para a porta interna do contêiner (80) e aguarda a iniciaização do backend
    . Criei um volume no disco rígido para garantir que as informações gravadas no banco não sejam apagadas se o contêiner for desligado

  
  <img width="886" height="1098" alt="image" src="https://github.com/user-attachments/assets/67f30410-1006-4f74-af64-6f559c579366" />


-> Realizado a Build para subir tudo 
      
      . Realizado o comando que leu o arquivo compose.yml e liga toda a infrastrutura : docker compose up
      . Realizado o comando que força o docker a contruir as imagens do backend e frontend dos Dockerfiles criados : --build
      . Realizado o comando que roda tudo em segundo plano no terminal : -d

      
<img width="886" height="37" alt="image" src="https://github.com/user-attachments/assets/32ff34f7-536f-492a-b362-c85d9822ed8c" />


-> Realizado a validação 
    
    . Após executar os comandos da build foi aberto o navegador pelo link: http://ec2-44-204-154-9.compute-1.amazonaws.com/

  <img width="886" height="243" alt="image" src="https://github.com/user-attachments/assets/852ccf05-0af7-4d83-9f60-02dde8b2e170" />





  DOCUMENTAÇÃO DE EXECUÇÃO DO LABORATÓRIO AWS DOCKER

-> A aplicação é estruturada em três camadas isoladas em contêineres Docker interconectados pela mesma rede virtual

-> Visão geral da Arquitetura 

<img width="788" height="597" alt="image" src="https://github.com/user-attachments/assets/a130536a-5648-4c07-bd46-c1dde1e76fa4" />


-> Estrutura de Arquivos de Configuração 

  -> Backend
  
 <img width="549" height="223" alt="image" src="https://github.com/user-attachments/assets/175b4725-b65b-4830-9f86-637bbb420415" />

-> Frontend 

<img width="481" height="195" alt="image" src="https://github.com/user-attachments/assets/cb19e585-ec84-4865-9da9-4989f8038f3a" />

-> Orquestração 


<img width="414" height="698" alt="image" src="https://github.com/user-attachments/assets/4cd75e74-a892-4b76-8ade-52a53305e7e3" />




<img width="886" height="999" alt="image" src="https://github.com/user-attachments/assets/eae49f79-e561-48f8-af7e-153cb1538e9c" />


->  Agendamento pra restartar o serviço do docker e gravar um log do horário que foi restartado o container

-> Realizei a conexão da SSH via PowerShell e estabeleci a sessão remota com a instância AWS EC2

<img width="886" height="999" alt="image" src="https://github.com/user-attachments/assets/04f18ad2-5638-4b38-bbcb-0e991a50995d" />

-> Abri o editor de texto para criar o script de automação 

<img width="886" height="38" alt="image" src="https://github.com/user-attachments/assets/3f740635-9ede-4ee6-821c-ecd703e67577" />

->  Realizado o script 
      
      . Defini o interpretador do script como Bash 
      . Criei a variavel que aponta para o camninho onde o log será guardado
      . Entrei no diretorio onde o ficheiro compose.yml da aplicação está localizado
      . Adicionei uma linha divisória e o timestamp de início ao final do ficheiro de log
      . Reiniciei o serviço do Docker no sistema e redirecionei todas as saidas para o log
      . Subi os contêineres em segundo plano (-d) e registrei a saida do log
      . Registrei a confirmação de sucesso como o horário final do log
      
  <img width="886" height="372" alt="image" src="https://github.com/user-attachments/assets/0e997b6b-8403-4418-88c7-91b7219b202d" />

-> Concedi a permissão de execussão ao ficheiro do script

<img width="886" height="39" alt="image" src="https://github.com/user-attachments/assets/051ddae5-8fe8-4abf-a4cf-bb57d3e22d6f" />

-> Abri a tabela de agendamentos para adicionar a regra de automação 

<img width="691" height="41" alt="image" src="https://github.com/user-attachments/assets/05c382fb-462b-4976-8d88-2e844fcc39ce" />

<img width="886" height="621" alt="image" src="https://github.com/user-attachments/assets/d09bbc06-6d8c-4016-bdb1-b14a48a83856" />

-> Executei o script para testar se ele funciona corretamente 

<img width="863" height="41" alt="image" src="https://github.com/user-attachments/assets/22012772-bce4-476b-a6c1-5f338104957c" />

-> Exibi o conteúdo do log para vallidar se o script escreveu as mensagens

<img width="886" height="333" alt="image" src="https://github.com/user-attachments/assets/d1216680-3264-4048-b7ac-d5930232f33b" />

-> Realizei a troca de agendamento 

    . O agendamento estava para ser de 5 em 5 minutos, troquei para que seja uma vez por dia à meia noite
    . Abri o agendador no terminal 
    . Alterei a linha que estava com */5 * * * * para 0 0 * * *
    . Validei que houve a troca de reiniciar de 5 em 5 minutos para uma vez por dia 

  <img width="393" height="30" alt="image" src="https://github.com/user-attachments/assets/df6a6285-93eb-4f67-b788-eca9d8616f7d" />

<img width="886" height="570" alt="image" src="https://github.com/user-attachments/assets/c6c78fde-098e-4f25-8446-6820787b985a" />

<img width="886" height="549" alt="image" src="https://github.com/user-attachments/assets/78f1fba8-482b-4166-8726-f5bbd3272f78" />




