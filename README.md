# Projeto: Automação do Ciclo Proposta → Projeto (n8n + ClickUp)

## 1. Visão Geral e Objetivo
* **Objetivo do Processo:** Automatizar a transição entre o fechamento comercial de um projeto em uma Empresa Júnior (EJ) de consultoria e a sua respectiva inicialização operacional (handover), eliminando a redigitação de dados, reduzindo o tempo de elaboração de propostas e garantindo padronização na entrega sem perder a customização inerente aos serviços da EJ.
* **Hipótese e Métricas:** A automação visa reduzir o tempo de trabalho manual e o tempo de ciclo da proposta e do handover, diminuindo também os erros de padronização. Serão comparados, antes e depois, em casos reais da EJ piloto: 
  1. Tempo de trabalho e tempo de ciclo da proposta.
  2. Tempo de handover até o projeto estar pronto no Drive e no ClickUp.
  3. Retrabalho e erros de padronização.
  4. Porcentagem de tarefas da EAP gerada pela IA que o gerente precisou ajustar. 
  *(Nota: Como a amostra inicial será pequena, os resultados serão apresentados como piloto).*

---

## 2. Cenário Atual (*As-Is*)
Atualmente, o processo sofre com gargalos manuais e dispersão de informações:
* **Pré-venda / Proposta:** O comercial preenche um formulário no ClickUp. Com base em estimativas da equipe, uma pessoa lê esses dados e gasta horas estruturando manualmente a EAP, calculando prazos/orçamentos e montando a apresentação comercial em slides. 
  * *Tempo de Trabalho (esforço efetivo):* Estimado em algumas horas de dedicação humana.
  * *Tempo de Ciclo (dias corridos, incluindo filas de espera e revisões):* Historicamente leva de 3 a 4 dias.
* **Pós-venda / Handover:** Após o projeto ser marcado como vendido, as informações do cliente são repassadas manualmente. Alguém cria a estrutura de pastas no Google Drive do zero, copia modelos de documentos manualmente, monta a estrutura de tarefas no ClickUp e aloca as equipes, gerando um tempo de ciclo operacional considerável e margem para despadronização.

---

## 3. Cenário Proposto (*To-Be*)
O novo ecossistema é composto por três fluxos desacoplados no n8n, conectados pelo status e estruturados através do card/tarefa do ClickUp como memória central:
* **Fluxo 1 (EAP Inteligente):** Disparado pelo envio do formulário comercial. O n8n envia os dados para uma IA estruturar a EAP e o escopo base. As horas vêm da IA, mas o preço final é calculado de forma determinística por uma regra de negócio própria em um nó Code (horas × valor/hora × margem). O resultado é salvo no card do ClickUp.
* **Fluxo 2 (Geração de Slide):** Disparado após a EAP ser validada e aprovada por um gerente (que revisa e ajusta horas/tarefas diretamente no ClickUp). O fluxo duplica o template corporativo no Google Slides, preenche os *placeholders* dinamicamente e entrega o material pronto ao comercial.
* **Fluxo 3 (Provisionamento Automático e Alocação):** Disparado quando o status do projeto muda para "Vendido". Cria automaticamente a estrutura de pastas e cópia de templates no Google Drive, instancia a estrutura de gestão no ClickUp com base na EAP, aloca as equipes e notifica os envolvidos.

---

## 4. Arquitetura Técnica e Decisões de Projeto

### Stack Tecnológica
* **Orquestrador:** `n8n` (Webhooks, nós de Code em JavaScript e requisições HTTP).
* **Gestão Comercial e Tarefas:** `ClickUp` (Formulário de entrada, armazenamento de estado/memória e gestão de operações).
* **Inteligência Artificial:** `Google Gemini` ou `Groq` (processamento de linguagem natural para geração da EAP).
* **Documentos:** `Google Drive` e `Google Slides` (armazenamento e automação de apresentações).

### Cuidados de Segurança e Governança com a IA
* Como planos gratuitos de IA possuem limites e políticas próprias de dados, o projeto adota o princípio de **anonimização de dados sensíveis de clientes** da EJ antes do envio para APIs externas, tratando essa como uma decisão consciente de arquitetura para portfólio.

### Riscos e Tratamento de Erros
* **JSON Inválido da IA:** Tratamento de falhas no nó Code para garantir fallback caso a IA retorne texto fora do padrão estruturado.
* **Disparos Duplicados de Status:** Validação de estados no ClickUp para evitar a criação duplicada de pastas no Google Drive.
* **Falhas de Integração:** Monitoramento de logs no n8n.

### Fases de Desenvolvimento
* **Fase 1 (MVP):** Construção do Fluxo 3 (Provisionamento de Pós-Venda) e Fluxo 1 (EAP com IA e regra de preço).
* **Fase 2 (Evolução):** Automação do Google Slides (Fluxo 2) e medição oficial da linha de base com a EJ piloto.
