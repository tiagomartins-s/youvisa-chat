# Plataforma Inteligente de Atendimento Multicanal - YOUVISA

Este projeto propõe uma solução de atendimento digital multicanal utilizando processamento de linguagem natural, automação de processos e integração entre sistemas, com o objetivo de otimizar o fluxo de comunicação entre usuários e a YOUVISA. O foco está em melhorar a eficiência operacional, reduzir tempo de atendimento e garantir consistência e segurança nas interações, oferecendo uma experiência fluida entre canais automatizados e atendimento humano quando necessário.

---

## 🧩 1. Justificativa do Problema

Atendimentos consulares e de emissão de vistos envolvem grande volume de solicitações repetitivas, dúvidas frequentes, envio de documentos e marcações de processos. Quando esse atendimento é realizado manualmente, surgem problemas como:

- Filas de atendimento extensas;
- Dificuldade de manter histórico e contexto da conversa;
- Informações incoerentes entre atendentes e canais;
- Alto custo operacional e retrabalho.

A YOUVISA busca **automatizar e centralizar** essas interações, garantindo que:

- O usuário possa iniciar o atendimento em um canal (WhatsApp, Telegram, Web, E-mail) e continuar em outro sem perder contexto;
- A maioria das dúvidas e tarefas simples seja resolvida automaticamente;
- Apenas casos necessários sejam encaminhados ao atendimento humano;
- A gestão do atendimento seja monitorada em tempo real através de um painel administrativo.

---

## 🧠 2. Descrição da Solução Proposta

Desenvolvemos uma **plataforma inteligente de atendimento** que integra:

- **Chatbot Multicanal** para interação com o usuário;
- **Classificador de Intenções (NLU)** para interpretar o que o usuário deseja;
- **Gerenciador de Slots (Slot Manager)** para identificar se dados adicionais são necessários para executar ações;
- **Módulo de Execução de Ações**, capaz de realizar operações como:
  - Agendar atendimentos,
  - Consultar status de processos,
  - Listar documentos necessários,
  - Atualizar cadastros.
- **Encaminhamento para Atendente Humano**, quando o usuário solicita ou quando há necessidade no atendimento.
- **Painel Web** para visualização das conversas em tempo real, métricas e gestão de fila de atendimento.

---

## 🧱 3. Tecnologias Utilizadas

| Componente | Tecnologias | Justificativa |
|---|---|---|
| Backend (Chatbot & Lógica) | **Python + FastAPI** | Leve, rápido e compatível com AWS Lambda |
| Classificador de Intenções (NLU) | **GPT-4o-mini (OpenAI)** | Alta precisão e baixo custo |
| Integração Multicanal | **WhatsApp Cloud API, Telegram Bot API, Web Chat** | Comunicação centralizada |
| Processamento Serverless | **AWS Lambda + API Gateway (REST & WebSocket)** | Escalabilidade automática |
| Persistência de Dados | **AWS DynamoDB** | Armazenamento orientado a conversação com baixo custo |
| Painel Web | **React + AWS S3 + CloudFront + Cognito** | Interface segura, rápida e responsiva |
| Streaming e Atualizações em Tempo Real | **DynamoDB Streams + EventBridge + SQS + API Gateway WebSocket** | Envio de eventos ao painel humano sem recarregar página |

---

## 🏗️ 4. Arquitetura da Solução (Resumo do Fluxo)

1. Usuário envia mensagem por **WhatsApp, Telegram ou Web Chat**.
2. A mensagem chega ao **API Gateway**, que aciona o **AWS Lambda**.
3. O backend envia a mensagem ao **Classificador de Intenções**, que identifica:
   - O que o usuário quer (intenção);
   - Dados que temos mapeados (Slots);
   - Dados adicionais (Slots não estruturados);
4. O sistema gera a **Resposta Rápida**:
   - Em caso de não haver inteções é a resposta final desse fluxo;
   - Em caso de ter que executar ações essa mensagem é apenas um aviso de que está trabalhando no que foi pedido;
5. Caso existam ações são executadas as **Validações**:
   - Em caso de precisar de informações adicionais para executar a ação, a mesma é posta em holding e as informações são requisitadas;
   - Em caso de precisar de confirmação de que o usuário deseja executar essa tarefa, a mesma é posta em holding e as informações são requisitadas;
6. Quando a ação é concluída, o resultado é salvo no **DynamoDB**.
7. O DynamoDB gera um evento para **Stream → EventBridge → SQS → WebSocket** e **Stream → EventBridge → SQS → API Gateway**.
8. O **Painel React** recebe **atualização em tempo real** da conversa, assim como o **WhatsApp, Telegram ou Web Chat** recebem a resposta.
9. Se necessário, um atendente humano **assume o atendimento** diretamente no painel.

---

## 🗄️ 5. Estratégia de Coleta e Tratamento de Dados

- **Histórico de conversas**, intenções detectadas, ações realizadas e dados do usuário são armazenadas no **DynamoDB**.
- Dados sensíveis (ex.: CPF) são tratados conforme **LGPD**:
  - Criptografia com **KMS** quando necessário;
  - Logs não expõem informações sensíveis;
  - Política de acesso baseada em perfis (ex.: atendentes vs. administradores).
- Dados servirão futuramente para:
  - Melhorar o classificador de intenções,
  - Gerar relatórios analíticos.

---

## 📝 6. Plano de Desenvolvimento & Divisão de Responsabilidades

| Responsáveis | Entrega |
|---|---|
| Tiago | Backend |
| Lucas | Frontend |
| Maurício | Infra |

1. MVP do projeto conectando com uma plataforma e executando uma ação.
2. Expansão de plataformas.
3. Criação da plataforma centralizada YOUVISA Chat.

---

## ✅ Status da Sprint Atual

Esta entrega corresponde à **Sprint 1**, contendo:

- Documentação da solução;
- Definição das tecnologias;
- Arquitetura completa da plataforma;
- Fluxo de atendimento com NLU e Slot Manager;
- Modelagem inicial de tratamento de dados.

---

## 📎 Diagrama da Arquitetura

> *([Diagrama YOUVISA Chat](https://drive.google.com/file/d/1yLK0OOqRGlZucxK-_iR8Q-NfLrYq_OTE/view?usp=sharing))*

---

## 🏁 Conclusão

A solução proposta atende aos objetivos do Challenge YOUVISA ao fornecer um modelo de atendimento digital inteligente, escalável, seguro e integrado, reduzindo esforço operacional, aumentando eficiência e melhorando a experiência do usuário.

---
