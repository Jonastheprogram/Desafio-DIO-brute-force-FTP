# Desafio-DIO-brute-force-FTP
Simulação de um ataque brute force em FTP utilizando duas VM's (máquina virtual). Uma delas com Kali Linux para fazer os ataques (sendo utilizado Medusa e Nmap como principais ferramentas) e a outra simulando um alvo vulnerável com o Metaesploitable 2.


# Conectando ao alvo 

Com o terminal do Kali Linux aberto utilizaremos o código ping -c 3 192.168.56.103 (ip do metaesploitable) para se conectar ao alvo.

# Enumeração de portas e serviços
Em seguida utilizaremos o comando nmap -sV -p 21,22,80,445,139 192.168.56.103 uma enumeração para verificar qual das portas estão abertas da máquina vulnerável, qual serviços e suas versões que estão sendo utilizadas.

o resultado esperado deve ser:

<img width="646" height="317" alt="enumeracao" src="https://github.com/user-attachments/assets/4f020f41-c456-4542-8f19-19cf6b4f7490" />

A porta 21 esta aberta e vunerável utilizando o servico FTP com a versão 2.3.4.

# Invadindo o sistema
Para fazer o teste de invasão precisamos descobrir as credencias do usuário para logar no sistema. Primeiramente iremos criar duas wordlists no terminal do Kali Linux.

Para criar um arquivo txt com lista de possíveis logins: echo -e "user\nmsfadmin\nadmin\nroot" > users.txt 
Para criar um lista de possiveis senhas: echo -e "123456\nnpassword\nqwery\nmsfadmin" > pass.txt  

Em seguida usaremos as listas criadas para realizar a busca pelo login correto do sistema, quando o comando for excutado o medusa ira pegar as duas listas e testar todas as combinações possíveis de uma só vez até encontrar uma combinação que de certo para entrar no sistema onde o -U do comando é para usuário e -P para senha.
medusa -h 192.168.56.103 -U user.txt -P pass.txt -M ftp -t 6

<img width="646" height="517" alt="login_medusa" src="https://github.com/user-attachments/assets/489c819f-a394-4f98-a24c-6bb34d1ef447" />

ao encontrar a combinação correta irá aparecer uma linha escrita com ACCOUNT FOUND no início e [SUCCESS] no final

agora com as informações de login poderá ser utilizado o comando ftp 192.168.56.103 e em seguida utilizar as credenciais encontradas pelo medusa para logar no sistema vulnerável.

<img width="530" height="189" alt="login_realizadoComSucesso" src="https://github.com/user-attachments/assets/6747a019-9313-4fe8-af63-1ba41d454610" />

Desta forma o FTP foi invadido de forma simples e rápida com apenas alguns comandos, apenas sabendo o endereço de ip de uma rede. O desafio busca lembrar a importância de manter boas práticas de cibersegurança hoje em dia utilizando outros métodos como FTPS e SFTP que são totalmento mais seguros a ataques de invasão como exemplo o brute force.
