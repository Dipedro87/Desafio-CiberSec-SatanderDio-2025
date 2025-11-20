# Desafio-CiberSec-SatanderDio-2025
 Simulando um Ataque de Brute Force de Senhas com Medusa e Kali Linux

Este repositório documenta a execução sobre a criação de ataques de força bruta em um ambiente de laboratório controlado. O objetivo foi explorar vulnerabilidades em diferentes serviços (FTP, SMB, Web) usando Kali Linux e ferramentas de auditoria.

1. 🛠️ Configuração do Ambiente
O laboratório foi construído no VirtualBox, simulando uma rede corporativa interna.

Máquina Atacante (Kali Linux):
IP (Host-Only): 192.168.56.101
Máquina Alvo (Metasploitable 2):
IP (Host-Only): 192.168.56.102

Detalhe Crítico da Rede (2 Placas)
Um dos principais desafios na configuração foi permitir que o Kali atacasse a rede interna (Host-Only) e, ao mesmo tempo, tivesse acesso à internet (NAT) para instalar pacotes (apt update, apt install).

A solução foi configurar duas placas de rede na VM do Kali:

Placa 1 (NAT): Para acesso à Internet.
Placa 2 (Rede Exclusiva do Hospedeiro): Para a rede interna do laboratório, permitindo a comunicação com o Metasploitable.
![Rede1ATTK](https://github.com/user-attachments/assets/c1f04427-4b8e-4396-b7d8-74d67c516ab4)

![Rede2HostOnly](https://github.com/user-attachments/assets/8b7cb709-b606-4cc3-a8f0-61c5b5ca69d5)

2. 📝 Preparação (Wordlists)
Listas de palavras simples foram criadas no Kali para realizar os testes de força bruta:

# Criando a lista de usuários
└─$ echo -e 'user\nmsfadmin\nadmin\nroot' > users.txt  

# Criando a lista de senhas
└─$ echo -e '123456\npassword\nqwerty\nmfsadmin' > pass.txt

![Wordlist](https://github.com/user-attachments/assets/f95da0e6-b0e3-4c24-bbe1-666aca802206)

3. 💥 Execução dos Ataques
Com o ambiente e as wordlists prontas, os seguintes cenários foram executados.

Cenário 1: Força Bruta no Serviço FTP (Medusa) O primeiro teste teve como alvo o serviço FTP (Porta 21) no Metasploitable, que estava ativo e exposto.

Ferramenta: Medusa

Comando: 
└─$ medusa -h 192.168.56.102 -U users.txt -P pass.txt -M ftp -t 6

Resultado e Evidência: O Medusa testou as combinações e rapidamente encontrou as credenciais corretas para o usuário msfadmin.
![AttkFTP](https://github.com/user-attachments/assets/bb646bf0-f2e3-4f74-9d23-804de2d151c1)

Cenário 2: Password Spraying em SMB (Medusa) O segundo teste foi um ataque de Password Spraying contra o serviço SMB (Porta 445). O objetivo era testar uma única senha comum (msfadmin) contra toda a lista de usuários.

Ferramenta: Medusa

Comando:

medusa -h 192.168.56.101 -U users.txt -p 'msfadmin' -M smbnt

Resultado e Evidência: O Medusa testou a senha msfadmin contra todos os usuários e encontrou um acesso válido.
![AttkFTP2](https://github.com/user-attachments/assets/e7d98df3-02ec-48c9-bb40-09fb9be99e5c)


