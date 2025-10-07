![GitHub last commit](https://img.shields.io/github/last-commit/ViniciusH97/Analise-de-vulnerabilidades-Metasploitable2)

<img width="668" height="199" alt="image" src="https://github.com/user-attachments/assets/bc40de8a-736a-4599-8811-052e4f697373" />

# Projeto Metasploitable2 Pentest com Kali Linux

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

**Este projeto é apenas para fins educacionais em ambiente controlado.  
Não utilize essas técnicas aqui descritas em sistemas ou redes sem autorização.  
O autor não se responsabiliza por usos indevidos.**


