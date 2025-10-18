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
| `files/passwd.txt` | Lista de senhas usada nos ataques (Wordlist). |
| `files/user.txt` | Lista de usuários usada nos ataques. |

### 🖼️ Arquivos na Pasta `imagens/`

| Nome da Imagem | Descrição |
| :--- | :--- |
| `medusa_command.png` | Captura de tela do comando Medusa sendo executado. |


### 📄 Arquivos na Pasta `files/`

| Nome do Arquivo | Uso |
| :--- | :--- |
| `passwd.txt` | Lista de senhas (`-P`) utilizada pelo Medusa. |
| `user.txt` | Lista de usuários (`-U`) utilizada pelo Medusa. |
| `ftp-login-success.png` | Confirma o login no serviço ftp. |

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
medusa -H 10.147.16.50 -u files/user.txt -p files/passwd.txt -M ftp
```

| Argumento | Descrição |
| :--- | :--- |
| `-H 10.147.16.50` | Define o Host alvo do ataque (IP do servidor). |
| `-u files/user.txt` | Especifica o arquivo contendo a lista de Usuários. |
| `-p files/passwd.txt` | Especifica o arquivo contendo a lista de Senhas. |
| `-M ftp` | Define o Módulo do serviço a ser atacado (FTP). |

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



