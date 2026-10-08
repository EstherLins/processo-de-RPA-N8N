# Projeto: Automação do Ciclo Proposta → Projeto (n8n + ClickUp)

## 1. Visão Geral e Objetivo
* **Objetivo do Processo:** Automatizar a transição entre o fechamento comercial de um projeto em uma Empresa Júnior (EJ) de consultoria e a sua respectiva inicialização operacional (handover), eliminando a redigitação de dados, reduzindo o tempo de elaboração de propostas e garantindo padronização na entrega sem perder a customização inerente aos serviços da EJ.
* **Resultado a Medir:** Redução drástica do tempo do ciclo de ponta a ponta (metrificando o *antes* e o *depois* com dados reais da EJ piloto).

---

## 2. Cenário Atual (*As-Is*)
Atualmente, o processo sofre com gargalos manuais e dispersão de informações:
1. **Pré-venda / Proposta:** O comercial preenche um formulário no ClickUp. Uma pessoa da equipe precisa ler esses dados, estruturar manualmente a EAP (Estrutura Analítica do Projeto), calcular prazos/orçamentos e montar a apresentação comercial em slides. Esse fluxo costuma levar **de 3 a 4 dias**.
2. **Pós-venda / Handover:** Após o projeto ser marcado como vendido, as informações do cliente precisam ser repassadas manualmente para a área de projetos. Alguém cria a estrutura de pastas no Google Drive do zero, copia modelos de documentos manualmente, monta a estrutura de tarefas no ClickUp e aloca as equipes, o que abre margem para erros humanos e despadronização.

---

## 3. Cenário Proposto (*To-Be*)
O novo fluxo é composto por fluxos integrados no n8n que conversam entre si:
* **Fluxo 1 (EAP Inteligente):** Gatilho no envio do formulário comercial $\rightarrow$ Tratamento dos dados $\rightarrow$ Geração da EAP estruturada e minuta de escopo via IA $\rightarrow$ Atualização do card no ClickUp.
* **Fluxo 2 (Geração de Slide):** Gatilho após a EAP aprovada/validada $\rightarrow$ Duplicação do template corporativo no Google Slides preenchendo os *placeholders* dinamicamente $\rightarrow$ Envio do material pronto para o comercial.
* **Fluxo 3 (Provisionamento Automático):** Gatilho quando o status do projeto muda para "Vendido" $\rightarrow$ Criação automática da estrutura de pastas e cópia de templates no Google Drive $\rightarrow$ Criação da estrutura de gestão e tarefas no ClickUp com base na EAP $\rightarrow$ Notificação automática da equipe de projetos.

---

## 4. Arquitetura Técnica End-to-End (Stack Tecnológica)
* **Orquestrador:** `n8n` (com uso de nós de Webhook, Code em JavaScript e HTTP Request).
* **Gestão Comercial e Tarefas:** `ClickUp` (utilizado como formulário de entrada, banco de memória entre os fluxos e ferramenta de gestão operacional).
* **Inteligência Artificial:** `Google Gemini` ou `Groq` (para processamento de linguagem natural e estruturação da EAP/proposta de forma gratuita e rápida).
* **Documentos e Apresentações:** `Google Drive` e `Google Slides` (para armazenamento em nuvem e automação de templates de propostas e relatórios).
