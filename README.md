![GitHub last commit](https://img.shields.io/github/last-commit/ViniciusH97/Analise-de-vulnerabilidades-Metasploitable2)

<img width="668" height="199" alt="image" src="https://github.com/user-attachments/assets/bc40de8a-736a-4599-8811-052e4f697373" />

# Projeto Análise de Vulnerabilidades Metasploitable 2 com Kali Linux (Pentest)

## Qual é o objetivo do repositório?

Este repositório documenta **passo a passo** a montagem de um laboratório de Pentest e os testes de invasão realizados contra a VM vulnerável **Metasploitable2**, utilizando o **Kali Linux** como máquina atacante.

- Escopo: configuração de duas VMs no VirtualBox em rede host-only, reconhecimento de serviços, ataques de força bruta e password spraying, validação de acessos, e recomendações de mitigação.
- Resultados esperados: demonstração reprodutível dos passos, comandos, wordlists utilizadas e análise de riscos e correções aplicáveis.

## Ambiente e Topologia
- Hypervisor: Oracle VirtualBox.
- VM Atacante: Kali Linux (última versão estável disponível no laboratório).
- VM Alvo: Metasploitable2.
- Rede: Host-only / Internal Network para isolamento do laboratório.
- Endereços:
- Kali: 192.168.56.102;
- Metasploitable2: 192.168.56.101.
  
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

**Este projeto é apenas para fins educacionais em ambiente controlado.  
Não utilize essas técnicas aqui descritas em sistemas ou redes sem autorização.  
O autor não se responsabiliza por usos indevidos.**

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
ping -c 4 192.168.56.101
```
<img width="765" height="498" alt="image" src="https://github.com/user-attachments/assets/67f96c36-473f-474a-b7a7-a214a21da3f8" />

## Reconhecimento das portas com Nmap

Para reconhecimento padrão vamos executar o comando `nmap` [endereço ip do metaspliotable2]. 

Exemplo: 

```bash
nmap 192.168.56.101
```

<img width="745" height="538" alt="image" src="https://github.com/user-attachments/assets/e3ed9481-b651-4ea7-a9da-59d186f0f10a" />

Vamos focar nas portas 21, 22, 80, 445 e 139 com o comando:

```bash
nmap -sV -p 21,22,80,445,139 192.168.56.101
```

<img width="735" height="295" alt="image" src="https://github.com/user-attachments/assets/61c76e11-4ff3-46df-8fbd-0b6cf5dd75ed" />

## Teste na porta FTP

Pode-se identificar a porta 21 (FTP), onde a mesma é utilizada para tranferência de arquivos. Nesse protocolo é possível realizar o download ou upload de arquivos do computador.

### Identificação da porta utilizando o Nmap

<img width="679" height="376" alt="nmap_metasploitable2" src="https://github.com/user-attachments/assets/ae411fd4-2725-4464-ad09-674510b33e60" />
