# BruteForceMedusa

## 🛡️ Exercício de Força Bruta com Medusa

Este repositório documenta um exercício prático de ataques de força bruta realizados em um ambiente controlado usando a ferramenta **Medusa** no Kali Linux. O objetivo foi testar a segurança de diferentes serviços (FTP e SMB) contra ataques de dicionário e força bruta.

---

## 🛠️ Detalhes do Ambiente e Arquivos

### 📂 Estrutura do Repositório

| Caminho | Descrição |
| :--- | :--- |
| `README.md` | Este arquivo de documentação. |
| `imagens/` | Contém capturas de tela do processo de ataque. |
| `files/` | Contém as listas de palavras (`wordlists`) e os arquivos de resultado gerados pelo Medusa. |

### 🖼️ Arquivos na Pasta `imagens/`

| Nome da Imagem | Descrição |
| :--- | :--- |
| `medusa_command.png` | Captura de tela do comando Medusa sendo executado. |
| `criando-wordlists.png` | Criando wordlist com o comando echo. |
| `enum4linux.png` | Captura de tela do comando enum4linux executado. |
| `enum4linux-users.png` | Captura de tela do comando enum4linux listando os usuários. |
| `ftp-failed.png` | Captura de tela do comando ftp com falha no login. |
| `ftp-login-succes.png` | Captura de tela do comando ftp com suscesso no login. |
| `medusa-bf-ftp.png` | Captura de tela do comando Medusa explorando o serviço ftp. |
| `medusa-spaying-users-passwords.png` | Captura de tela do comando Medusa sendo executado. |
| `Outros ` | Outros arquivos que comprovam a exploração do alvo. |

### 📄 Arquivos na Pasta `files/`

| Nome do Arquivo | Uso |
| :--- | :--- |
| `pass.txt` | Lista de senhas (`-P`) utilizada pelo Medusa. |
| `users.txt` | Lista de usuários (`-U`) utilizada pelo Medusa. |
| `enum4_output` | Saida do comando enum4linux. |
| `senhas_spray.txt` | Lusta de senhas spray. |
| `smb_users` | Lista de usuários obtidas pelo comando enum4linux. |
---

## 📝 Passo a Passo dos Ataques

Os ataques foram executados em sequência através do terminal Zsh no Kali Linux.
Primeiro, utilizando o nmap, foi pesquisado os hosts ativos na rede, comando:
```bash
nmap -sn 10.147.16.0/24
```
Com o alvo identificado, utilizamos o comando ping para confirmar se o host respondia, confirmando a execução.
Mais uma vez, utilizamos o comando nmap para verificar as portas e as versões dos serviços rodando no alvo, que pode ser compravado com a imagem nmap-sV.png

```bash
nmap -sV -p 21,22,80,445,139 10.147.16.11 
```
### 1. Ataque de Força Bruta ao Serviço FTP

O primeiro alvo foi o serviço FTP (File Transfer Protocol).

**Comando:**
```bash
medusa -h 10.147.16.50 -U files/user.txt -P files/passwd.txt -M ftp -t 6
```
| Argumento | Descrição |
| :--- | :--- |
| `-h 10.147.16.50` | Define o Host alvo do ataque (IP do servidor). |
| `-U files/user.txt` | Especifica o arquivo contendo a lista de Usuários. |
| `-P files/passwd.txt` | Especifica o arquivo contendo a lista de Senhas. |
| `-M ftp` | Define o Módulo do serviço a ser atacado (FTP). |
| `-t 6` | Define a quantidade de theads, torna o ataque mais rápido. |

### 1.1 Resultado do ataque ao ftp

Foi encontrado um usuário e senha válidos: usuário: msfadmin, senha: msfadmin

### 2. Enumeração de Usuários e Ataque de Força Bruta ao Serviço SMB

O foco foi direcionado ao serviço SMB (Server Message Block). Diferente do ataque FTP, este ataque foi precedido por uma etapa de enumeração de usuários.

#### 2.1 Enumeração com `enum4linux`

Primeiramente, a ferramenta `enum4linux` foi utilizada para listar informações do alvo, o que ajudou a coletar uma lista inicial de usuários.

**Comando:**
```bash
enum4linux 10.147.16.50 | tee files/enum4l_output.txt
```

### 📄 Arquivos na Pasta `files/`

| Nome do Arquivo | Uso |
| :--- | :--- |
| `enum4_output.txt` | Arquivo gerado pelo comando enum4linux. |
| `smb_users.txt` | Lista de usuários utilizada pelo Medusa. |
| `senhas_spray.txt` | Lista de senhas utilizada pelo Medusa. |
| `medusa_success.png` | Captura de tela mostrando o resultado bem-sucedido da força bruta (SMB). |
| `smbclient.png` | Confirma o login no cliente smb. |

### 2.1 Resultado da exploração do serviço SMB

Com o resultado do comando enu4linux, montamos a wordlists, para passar para o comando medusa:

```bash
medusa -h 10.147.16.50 -U files/smb_users.txt -P files/senhas_spray.txt -M smbnt -t 2 -T 50
```

O comando encontrou o usuário válido, msfadmin, com a senha msfadmin, o qual utilizamos para conectar no cliente smb, como evidência a imagem smbclient.png.

#### 3 Resultado do exercício

Foi demostrado aqui, por meio de arquivos obtidos, prints do terminal da máquina que executou os ataques (Kali Linux), a comprovação da exploração dos serviços de ftp e smb, do alvo, por meio das ferramentas nmap, ping, enum4linux e medusa.

###🛑 Disclaimer
Este material tem caráter estritamente educacional e de teste de segurança em ambientes controlados e autorizados. A realização de ataques de força bruta ou qualquer atividade de hacking sem a devida permissão é ilegal e antiética. Utilize este conhecimento de forma responsável.

