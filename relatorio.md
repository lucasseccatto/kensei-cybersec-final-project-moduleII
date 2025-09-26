# PROPOSTA DE CONSULTORIA EM SEGURANÇA CIBERNÉTICA
**Cliente:** LojaZeta  
**Autor:** Lucas Seccatto  
**Data:** 26/09/2025

---

## 1. Sumário Executivo

A LojaZeta, e-commerce em crescimento, utiliza a stack **Nginx + Node.js + PostgreSQL** hospedada em **infraestrutura em nuvem (IaaS)**. A exposição pública do web/app, combinada com incidentes recentes de **SQLi, XSS e brute-force no endpoint /login**, evidencia a necessidade de fortalecer a postura de segurança. A empresa possui logs dispersos, backups sem testes de restauração e um time enxuto (2 devs, 1 ops) com orçamento limitado.

**Riscos-chave:**
- Exposição pública da aplicação e infraestrutura web
- Falta de monitoramento centralizado e visibilidade de logs
- Ausência de testes de restauração de backups
- Resposta a incidentes reativa e pouco estruturada

**Visão de Solução:**
- Implementar **arquitetura de defesa em camadas**: WAF, segmentação de rede, hardening de hosts e aplicação, criptografia de dados e gestão de identidade
- Implantar **SIEM mínimo viável** para centralização e correlação de logs, com alertas acionáveis
- Estabelecer **procedimentos simplificados de resposta a incidentes** baseados no NIST IR
- Definir **plano 80/20** com **quick wins em 30 dias**

**Ganhos Esperados:**
- Redução da superfície de ataque
- Melhoria na detecção e resposta a incidentes
- Maior resiliência operacional e proteção da reputação da marca
- Retorno sobre investimento em segurança adaptado à realidade da LojaZeta

---

## 2. Escopo e Metodologia

**Escopo:**
- Arquitetura de defesa em camadas
- Monitoramento centralizado (SIEM mínimo viável)
- Resposta a incidentes
- Revisão da infraestrutura atual (Nginx, Node.js, PostgreSQL em IaaS) e identificação de riscos

**Metodologia:**
- Análise baseada em informações do briefing
- Priorização de **quick wins** e soluções de **mínimo viável**
- Recomendações escaláveis considerando equipe pequena e orçamento limitado

---

## 3. Arquitetura de Defesa em Camadas

**Diagrama sugerido:**

```
                        Internet
                            │
                         WAF/CRS
                            │
                    ┌─────────────┐
                    │Load Balancer│
                    └─────┬───────┘
                          │
          ┌───────────────┴───────────────┐
          │                               │
      App Node.js                     Coleta/Correlação
          │                               │
          ▼                               ▼
      PostgreSQL ─────────────────────> Logs
          ▲
          │
      Identidade
```
### 3.1 Camada de Perímetro
- **WAF:** Protege contra SQLi, XSS e brute-force

### 3.2 Camada de Rede
- Segmentação de rede: sub-redes privadas e VPCs para aplicação e banco de dados
- Load Balancer: distribuição de tráfego e camada extra de segurança

### 3.3 Camada de Host
- Hardening de servidores (Nginx, Node.js, PostgreSQL)
- Firewall de host e permissões corretas de arquivos
- Avaliação de IDS/IPS

### 3.4 Camada de Aplicação
- Validação de entrada e sanitização de dados
- Revisão de código focada em segurança
- Monitoramento de dependências e atualizações de bibliotecas

### 3.5 Camada de Dados
- Criptografia em repouso e trânsito
- Controle de acesso baseado no princípio do menor privilégio
- Testes periódicos de restauração de backups

### 3.6 Camada de Identidade e Acesso (IAM)
- Autenticação forte (MFA) para usuários privilegiados
- Controle de permissões baseado em funções e responsabilidades

---

## 4. Monitoramento & SIEM

**Fontes de Log:** Nginx, Node.js, PostgreSQL, SO Linux, WAF

**Coleta e Centralização:**
- Filebeat/rsyslog ou open-source como ELK Stack / Graylog
- Centralizar logs para visibilidade e correlação

**Correlação e Alertas (exemplos):**
- Tentativas de login falhas → alerta de brute-force
- SQLi/XSS detectado → alerta de injeção
- Acesso incomum ao DB → alerta de acesso suspeito
- Erros 5xx → alerta de indisponibilidade

**KPIs/Métricas:**
- **MTTD:** < 1 hora
- **MTTR:** < 4 horas
- **Cobertura de Logs:** 100% dos sistemas críticos
- **Tentativas bloqueadas:** aumentar taxa de bloqueio

---

## 5. Resposta a Incidentes (NIST IR)

**Fases:** Detecção → Análise → Contenção → Erradicação → Recuperação → Lições Aprendidas

**Runbooks Simplificados (exemplo):**
- **SQLi:**
  1. Identificar ataque via logs/WAF
  2. Bloquear IPs maliciosos
  3. Corrigir vulnerabilidade no código
  4. Validar integridade do banco de dados

- **XSS / Brute-Force / Indisponibilidade:** seguir lógica similar adaptada ao tipo de incidente

---

## 6. Recomendações 80/20 e Roadmap

**Quick Wins (30 dias – essencial)**
- Implementar WAF
- Centralizar logs essenciais
- Hardening básico
- Testes de restauração de backup
- MFA para acesso administrativo

**Médio Prazo (90 dias – essencial + bom ter)**
- Configurar SIEM mínimo viável
- Segmentação de rede
- Revisão de código para segurança
- Monitoramento de dependências

**Longo Prazo (180 dias – bom ter)**
- IDS/IPS
- Treinamento em segurança
- Automação de respostas de contenção
- Criptografia de dados em repouso
- Simulações de incidentes

---

## 7. Riscos, Custos e Assunções

**Riscos:**
- Recursos limitados
- Orçamento restrito
- Curva de aprendizado da equipe
- Falsos positivos/negativos nos alertas
- Possível interrupção temporária do serviço

**Custos:**
- Ferramentas (WAF, SIEM)
- Consultoria e treinamento
- Infraestrutura de nuvem para logs e SIEM
- Tempo da equipe

**Assunções:**
- Apoio da liderança
- Disponibilidade da equipe
- Acesso completo à infraestrutura
- Logs essenciais disponíveis
- Backups funcionais

---

## 8. Conclusão

Com esta proposta, a **LojaZeta** terá:
- Arquitetura em camadas robusta
- Monitoramento centralizado e alertas acionáveis
- Plano de resposta a incidentes simplificado
- Proteção de ativos e resiliência operacional

**Próximos Passos:**
1. Reunião de alinhamento com equipe e liderança
2. Planejamento detalhado do roadmap
3. Implementação inicial dos quick wins (30 dias)

**Critérios de Sucesso:**
- Redução de incidentes críticos
- Melhora nos KPIs (MTTD, MTTR, cobertura de logs)
- Engajamento da equipe em práticas de segurança
- Resiliência operacional da LojaZeta
