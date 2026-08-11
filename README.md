SOC Home Lab — Laboratório de Cibersegurança

Laboratório SOC desenvolvido para prática em Cibersegurança e Blue Team, simulando um ambiente corporativo com Kali Linux e Windows 11. Projeto focado em monitoramento, análise de logs, detecção de ameaças, investigação de incidentes e resposta a eventos de segurança.

Arquitetura do Lab
[Kali Linux] ---\
                  >--- Rede Interna "Lab SOC" (192.168.50.0/24) ---< [Windows Vitima]
[Ubuntu/Wazuh] --/                                                        |
                                                                  (futuro: agente Wazuh)
Kali Linux — máquina de ataque/reconhecimento
Windows Vitima — máquina alvo
Ubuntu Server + Wazuh (em andamento) — SIEM para coleta de logs e detecção de alertas
Todas as VMs isoladas em uma rede interna do VirtualBox ("Lab SOC"), sem acesso externo entre si além do próprio lab
Ferramentas Utilizadas
Oracle VirtualBox (virtualização)
Kali Linux 2026.2
Windows 11 Enterprise Evaluation
Nmap (reconhecimento de rede)
Wazuh (SIEM — em instalação)
Etapas Realizadas
1. Configuração da rede interna isolada

Foi criada uma rede interna no VirtualBox chamada "Lab SOC", conectando a Kali e o Windows em um segmento isolado, sem interferência da rede física do host.

Kali: Adaptador 1 em NAT (internet) + Adaptador 2 em Rede Interna "Lab SOC"
Windows Vitima: Adaptador 2 em Rede Interna "Lab SOC"

Mostrar Imagem Mostrar Imagem

2. Atribuição de IPs fixos

Como redes internas do VirtualBox não possuem DHCP, foram configurados IPs estáticos manualmente:

Kali: 192.168.50.10/24 (interface eth1)
  sudo ip addr add 192.168.50.10/24 dev eth1
Windows Vitima: 192.168.50.20/24 (configurado via TCP/IPv4 no adaptador de rede)

Mostrar Imagem Mostrar Imagem

3. Teste de conectividade

Conectividade entre as duas VMs confirmada via ping, validando que a rede interna está funcionando corretamente.

4. Reconhecimento inicial com Nmap

Comando executado a partir da Kali:

nmap -sV -A 192.168.50.20

Resultado: todas as 1000 portas escaneadas retornaram como filtered (nenhuma resposta), indicando que o Firewall do Windows Defender está bloqueando ativamente as sondagens de reconhecimento.

Análise: esse comportamento demonstra a eficácia da configuração padrão de firewall do Windows contra varreduras externas básicas — um firewall bem configurado não apenas fecha portas, mas evita até confirmar sua existência ao atacante.

Mostrar Imagem

Próximas Etapas
 Instalar Ubuntu Server e configurar o Wazuh (SIEM)
 Instalar o agente Wazuh no Windows
 Simular o primeiro ataque (ex: força bruta ou exploração de serviço)
 Capturar e analisar o primeiro alerta gerado no dashboard do Wazuh
 Documentar mitigação e recomendações finais

Última atualização: 11/08/2026
