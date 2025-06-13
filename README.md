# Certificação Microsoft Azure Administrator (AZ-104)

Este documento reúne anotações baseadas nas questões e tópicos do exame AZ-104, voltado para administradores do Microsoft Azure. São conceitos práticos e teóricos essenciais para dominar a administração de ambientes Azure em empresas.

---

## Introdução

O exame AZ-104 é destinado a profissionais que atuam como administradores de infraestrutura em ambientes Microsoft Azure, abrangendo desde o gerenciamento de identidades, redes, armazenamento, máquinas virtuais até a governança e conformidade.

Este documento sintetiza os principais conceitos, práticas e respostas às questões mais recorrentes em simulados e exames oficiais.

---

## 1. Administração de Identidade

- Azure Active Directory (Azure AD) é a base para autenticação, gerenciamento de usuários, grupos e aplicativos na nuvem.
- Autenticação multifator (MFA) e integração com apps SaaS.
- Configuração de usuários convidados, permissões via RBAC e licenciamento.
- Azure Bastion oferece acesso remoto seguro a VMs sem expor portas RDP/SSH.
- Desired State Configuration (DSC) para padronizar configurações em VMs Windows e Linux.
- **Monitoramento de logins e falhas de autenticação** usando Azure AD Sign-in logs no Log Analytics.

---

## 2. Administração de Governança e Conformidade

- Uso de Azure Policy para garantir conformidade e restrições.
- Monitoramento com Azure Monitor e Security Center.
- Controle de acesso privilegiado com Azure AD PIM.
- **Service Health** para acompanhar a saúde dos serviços Microsoft e receber notificações de indisponibilidade.
- **Alertas e Action Groups** configurados via Azure Monitor para violações de política.

---

## 3. Administração dos Recursos do Azure

- Organização em grupos de recursos.
- Implantação com ARM Templates, CLI e PowerShell.
- Automação com Azure Automation e Logic Apps.
- **Tags** para classificação e cobrança de recursos.
- **Blueprints** para replicar padrões de governança.

---

## 4. Administração de Rede Virtual

- Criação e configuração de Redes Virtuais (VNets), subnets e NSGs.
- Configuração de VPN Gateway e ExpressRoute.
- Balanceadores de carga: Azure Load Balancer e Application Gateway.
- **Network Watcher**:
  - Topologia de rede e IP Flow Verify.
  - Captura de pacotes (“packet capture”) e connection troubleshoot.
  - Logs de fluxo NSG analisados no Log Analytics.

---

## 5. Administração de Conectividade entre Sites

- VPN site-to-site para redes locais.
- ExpressRoute para conexão privada dedicada.
- **Point-to-Site VPN** para acesso de administradores.
- Monitoramento de túneis VPN com Network Watcher e alertas.

---

## 6. Administração do Tráfego de Rede

- Azure Traffic Manager para balanceamento global e failover.
- Azure Front Door para CDN e segurança.
- **Monitoramento de endpoints** configurado via probes e alertas de disponibilidade.

---

## 7. Administração do Armazenamento do Azure

- Diferenças entre contas Standard e Premium.
- Uso de SAS Tokens para acesso temporário seguro.
- Política de retenção de exclusão reversível para restaurar blobs.
- Ferramenta recomendada para transferência de dados pequenos: AZCopy.
- Protocolo para Azure Files: SMB 3.0 (porta 445).
- **Azure Backup Center** para unificar backups de VMs, arquivos e bases de dados.
- **Snapshots**:
  - Recuperação rápida sem restaurar backup completo.
  - Impacto em custos e armazenamento dimensionado automaticamente.

---

## 8. Administração de Máquinas Virtuais do Azure

- Escolha de disco, tamanho e configuração conforme demanda.
- Conjuntos de Disponibilidade (Availability Sets) para alta disponibilidade.
- Virtual Machine Scale Sets (VMSS) para escalonamento automático horizontal.
- Backup de VMs é responsabilidade do administrador.
- **Instantâneos gerenciados** para recuperação pontual de discos.
- **Agente MARS** para backup de máquinas físicas e VMs Linux/Windows no Recovery Services Vault.
- **Propriedades obrigatórias**:
  - Recovery Services Vault na mesma região da VM.
  - Agente ou extensão habilitada para snapshots.

---

## 9. Observabilidade e Monitoramento

- **Métricas vs Logs**  
  - Métricas: dados numéricos de performance (CPU, disco, rede).  
  - Logs: eventos descritivos, registros de atividade e erros.
- **Log Analytics Workspace**  
  - Repositório onde são agregados logs e métricas.  
  - Coleta de dados de VMs, Storage, NSGs, Network Watcher e OPNsense.
- **Kusto Query Language (KQL)**  
  - Linguagem para consultar e correlacionar tabelas como `Heartbeat`, `Syslog`, `SecurityEvent`, `AzureDiagnostics`.
- **Azure Monitor Insights**  
  - Insights for VMs: mapeia dependências de processos, performance de SO e disco.  
  - Network Insights: topologia, fluxos de tráfego e diagnósticos de VPN.
- **Alertas e Action Groups**  
  - Regras de alerta definem recurso, condição (limite de métrica ou query) e severidade.  
  - Action Groups enviam notificações por e-mail, SMS ou acionam Logic Apps/Functions.
- **Dashboards e Workbooks**  
  - Dashboards personalizados no portal Azure para visão geral.  
  - Workbooks interativos para relatórios de disponibilidade, utilização e incidentes.

---

# Projeto de Ambiente Azure com NextCloud, OPNsense e Integração Entra ID

Este ambiente oferece acesso seguro a arquivos via NextCloud, com firewall OPNsense e autenticação centralizada no Microsoft Entra ID.

## Arquitetura e Componentes

- **VM 1: OPNsense**  
  - IP interno: `10.0.2.1`  
  - Firewall, HAProxy (proxy reverso), ACME client para certs, OpenVPN.
- **VM 2: NextCloud**  
  - IP interno: `10.0.2.10`  
  - App NextCloud + banco de dados; arquivos em Azure Files (SMB).
- **Azure Files**  
  - Storage para repositório de dados do NextCloud.
- **Autenticação**  
  - SSO via SAML registrado no Microsoft Entra ID.
- **Certificados SSL**  
  - Gerados e renovados automaticamente no OPNsense.
- **Acesso Remoto**  
  - OpenVPN no OPNsense para RDP/SSH às VMs.

## Rede, Acesso e Firewall

| Origem           | Destino               | Porta   | Protocolo | Função                                |
|------------------|-----------------------|---------|-----------|---------------------------------------|
| Usuário Externo  | OPNsense (IP público) | 443     | HTTPS     | Acesso NextCloud via proxy reverso    |
| OPNsense         | VM NextCloud          | 80, 443 | HTTP/HTTPS| Comunicação interna NextCloud         |
| VM NextCloud     | Azure Files           | 445     | SMB       | Armazenamento de arquivos             |
| Usuário VPN      | VMs na VNet           | 3389/22 | TCP       | Acesso remoto via OpenVPN             |
| Network Watcher  | —                     | —       | —         | Monitoramento de rede habilitado      |

## Monitoramento e Observabilidade no Projeto

1. **Log Analytics Workspace**  
   - Criado no mesmo RG e região do projeto.  
   - Coleta dados de:  
     - VMs (Heartbeat, Insights for VMs).  
     - OPNsense (Syslog via agente ou coletor syslog).  
     - Azure Files (AzureDiagnostics).  
     - Network Watcher (NSG Flow Logs).

2. **Azure Monitor Insights**  
   - **VM Insights**: habilita diagnóstico de CPU, disco, rede e dependências de processo para NextCloud e OPNsense.  
   - **Network Insights**: visualiza topologia e falhas de conexão (IP Flow Verify, Connection Troubleshoot).

3. **Alertas e Action Groups**  
   - Exemplo de alertas configurados:  
     - **Disponibilidade NextCloud**: ping de heath probe falhando → severidade “Critical”.  
     - **Uso de armazenamento**: quota Azure Files > 80% → severidade “Warning”.  
     - **VPN Down**: monitorar saúde do túnel OpenVPN no OPNsense via syslog.  
   - **Action Group** notifica administradores por e-mail e envia mensagem ao Teams.

4. **Dashboards e Workbooks**  
   - **Dashboard “Ambiente NextCloud”**: widgets com CPU, memória e disco das VMs.  
   - **Workbook “Saúde da Rede”**: consulta KQL para fluxos bloqueados (NSG flow logs) e IP Flow Verify.  
   - **Workbook “Logs OPNsense”**: filtra erros de VPN, autenticações SAML e tráfego HTTP 5xx.

5. **Consultas KQL de exemplo**  
   ```kusto
   // CPU médio das VMs última hora
   InsightsMetrics
   | where Namespace == "Processor" and Name == "PercentProcessorTime"
   | summarize avg(Value) by Computer, bin(TimeGenerated, 1h)

   // Fluxos bloqueados por NSG no último dia
   AzureDiagnostics
   | where Category == "NetworkSecurityGroupFlowEvent" and FlowDirection_s == "I"
   | where Action_s == "Deny"
   | summarize count() by Resource_s, bin(TimeGenerated, 1h)
