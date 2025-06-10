
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
- Autenticação em VMs Linux via par de chaves SSH.
- Desired State Configuration (DSC) para padronizar configurações em VMs Windows e Linux.

---

## 2. Administração de Governança e Conformidade

- Uso de Azure Policy para garantir conformidade e restrições.
- Monitoramento com Azure Monitor e Security Center.
- Controle de acesso privilegiado com Azure AD PIM.

---

## 3. Administração dos Recursos do Azure

- Organização em grupos de recursos.
- Implantação com ARM Templates, CLI e PowerShell.
- Automação com Azure Automation e Logic Apps.

---

## 4. Administração de Rede Virtual

- Criação e configuração de Redes Virtuais (VNets), subnets e NSGs.
- Configuração de VPN Gateway e ExpressRoute.
- Balanceadores de carga: Azure Load Balancer e Application Gateway.

---

## 5. Administração de Conectividade entre Sites

- VPN site-to-site para redes locais.
- ExpressRoute para conexão privada dedicada.

---

## 6. Administração do Tráfego de Rede

- Azure Traffic Manager para balanceamento global e failover.
- Azure Front Door para CDN e segurança.

---

## 7. Administração do Armazenamento do Azure

- Diferenças entre contas Standard e Premium (Premium não suporta replicação geográfica).
- Uso de SAS Tokens para acesso temporário seguro.
- Política de retenção de exclusão reversível para restaurar blobs.
- Ferramenta recomendada para transferência de dados pequenos: AZCopy.
- Protocolo para Azure Files: SMB 3.0.
- Porta 445 deve estar liberada para acesso SMB.

---

## 8. Administração de Máquinas Virtuais do Azure

- Escolha de disco, tamanho e configuração conforme demanda.
- Conjuntos de Disponibilidade (Availability Sets) para alta disponibilidade.
- Virtual Machine Scale Sets (VMSS) para escalonamento automático horizontal.
- Regras baseadas em métricas acionam escalonamento automático (CPU, memória).
- Backup de VMs é responsabilidade do usuário.
- Durante manutenção planejada, mover VMs conforme instruções para evitar downtime.
- Configurações de rede padrão permitem tráfego de saída e restringem entrada.
- Ambiente de produção usa alta disponibilidade e backup, diferente do teste.

---

## Anotações Extras

- Tempo de inatividade inesperado ocorre por falha não planejada.
- Escalonamento vertical aumenta recursos de uma VM; horizontal adiciona mais VMs.
- Uso de Azure Bastion elimina exposição direta de portas RDP/SSH.
- Balanceadores de carga + Availability Sets reduzem impacto de falhas.
- Autenticação em VMs Linux deve usar par de chaves SSH.
- Backup das VMs deve ser configurado manualmente.

---

## Diagramas

Modelo de Conjunto de Disponibilidade:
```
Rack 1: VM1, VM3
Rack 2: VM2, VM4
Falha em Rack 1 -> VMs no Rack 2 continuam ativas
```

Fluxo de Acesso Azure Bastion:
```
Usuário -> Portal Azure -> Azure Bastion -> VM (RDP/SSH)
```
Sem expor portas direto à internet.

Escalonamento Automático (VMSS):
```
Métricas (CPU, Memória)
      ↓
Regras definem quando escalar ↑ ou ↓
      ↓
Criar/Remover instâncias VM automaticamente
```

---

## Referências

- [Microsoft AZ-104 Official Certification](https://learn.microsoft.com/pt-br/certifications/azure-administrator/)
- Documentação oficial Microsoft Azure
- Estudos e exercícios baseados em questões práticas do exame AZ-104

