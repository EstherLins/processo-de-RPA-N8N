# Automação do Ciclo Proposta → Projeto (n8n + ClickUp + Google Gemini + Google Workspace)

## 1. Visão Geral e Objetivo

Este projeto automatiza a transição entre o fechamento comercial de um serviço de consultoria e a sua execução operacional.

A solução elimina a digitação manual redundante, reduz drasticamente o tempo de resposta comercial (*lead time* da proposta) e garante rigor na padronização da entrega, mantendo a flexibilidade e personalização dos projetos.

### Métricas & Impacto Esperado

- **Redução do Tempo de Ciclo:** Diminuição no tempo total para elaborar propostas e preparar a estrutura operacional.
- **Eliminação de Esforço Manual:** Automação na criação de EAP, precificação, apresentações em Google Slides e estruturas no Google Drive.
- **Padronização:** Redução a zero de erros de digitação e despadronização de documentos.

## 2. Arquitetura do Sistema & Stack Tecnológica

- **Orquestrador de Fluxos:** `n8n` (arquitetura linear com validação de nós e parsing de dados).
- **Gestão Comercial e Operacional:** `ClickUp` (formulários de entrada, controle de status e memória central).
- **Inteligência Artificial:** `Google Gemini` (processamento de linguagem natural e estruturação da EAP/proposta via nós oficiais do LangChain/Google).
- **Documentação & Produtividade:** `Google Workspace` (Google Drive, Google Docs, Google Slides e Gmail).
- **Controle Financeiro/Precificação:** Regra determinística via nó Code / JavaScript (`Horas × Valor/Hora × Margem`).

## 3. Estrutura dos Fluxos de Automação (*To-Be*)

O sistema é dividido em fluxos desacoplados conectados pelo status do ClickUp:

```text
[Formulário Comercial]
       │
       ▼
1. Estruturação EAP & Precificação (Code + Google Gemini)
       │
       ▼
2. Aprovação do Gerente (ClickUp)
       │
       ▼
3. Geração de Apresentação (Google Slides) & Envio por E-mail (Gmail)
       │
       ▼
4. Status = "Vendido" ➔ Criação de pastas e documentos no drive e lista e tarefas no ClickUp
```

### Fluxo 1: Criação da proposta, com EAP, Cronograma e Precificação

- **Gatilho:** Encio do formulário comercial pelo cliente.
- **Processamento de IA:** Envio dos requisitos para o **Google Gemini** para estruturação da EAP, diagnóstico, cronograma e detalhamento do escopo.
- **Cálculo Determinístico:** As horas sugeridas pela IA passam por uma regra de negócio fixa para determinar o orçamento correto.
- **Saída:** Atualização do card central no ClickUp + planilha com a proposta.

### Fluxo 2: Geração de slide da Proposta Comercial & Envio Automatizado

- **Gatilho:** Aprovação manual e validação da proposta (EAP, Cronograma, e Precificação) pelo gerente no ClickUp.
- **Geração de Slides:** Duplicação do template oficial no Google Slides e preenchimento automático dos *placeholders* (diagnóstico, escopo, entregáveis e cronograma).
- **Notificação:** Envio automático de e-mail ao cliente/equipe via Gmail contendo o link da apresentação formatada.

### Fluxo 3: Criação da estrutura de gestão do projeto no drive e ClickUp

- **Gatilho:** Alteração do status do projeto para `"Vendido"`.
- **Google Drive:** Criação da estrutura de pastas da empresa (`00 - Comercial`, etc.) e cópia de planilhas/documentos base.
- **Gestão de Operações:** Criação automática da lista de tarefas e subtarefas no ClickUp com alocação dos membros responsáveis pela execução.

## 4. Governança, Segurança e Tratamento de Erros

- **Uso de IA Integrada:** Adoção do ecossistema nativo do Google Gemini no n8n para interpretação e geração de linguagem natural.
- **Anonimização de Dados (LGPD):** Remoção/substituição prévia de dados sensíveis e pessoais antes do envio do prompt para serviços de IA de terceiros.
- **Prevenção de Disparos Duplicados:** Checagem de estado e status no ClickUp para evitar a recriação redundante de pastas no Google Drive em reexecuções.
