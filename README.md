# 🪙 Tostão - O Seu Gestor Financeiro de Bolso

## Contexto

Organizar o fluxo de caixa pessoal é um desafio para a maioria das pessoas — entender 
para onde o dinheiro foi, o que cada produto financeiro significa e como tomar decisões 
mais conscientes sem precisar de um especialista.

O **Tostão** é um agente financeiro pessoal que utiliza IA Generativa para:

- **Analisar transações** e identificar padrões de gasto com base nos dados do usuário
- **Explicar conceitos** financeiros com linguagem simples, sem jargão de mercado
- **Organizar o orçamento** de forma didática e personalizada
- **Garantir respostas confiáveis** com guardrails contra alucinações e recomendações indevidas

---

## 1. 🎯 Caso de Uso

- **O Problema:** Muitas pessoas têm dificuldade em organizar o fluxo de caixa pessoal,
  entender para onde o dinheiro está indo e compreender conceitos financeiros sem o
  jargão do mercado.
- **A Solução:** O Tostão atua como um educador e organizador financeiro proativo.
  Ele analisa transações, explica conceitos de forma simples usando os próprios dados
  do usuário como exemplo prático — sem fornecer recomendações diretas de investimento.
- **Público-Alvo:** Pessoas que querem assumir o controle do orçamento, entender seus
  padrões de gasto e aprender a gerenciar seus recursos de forma estruturada.

---

## 2. 🗣️ Persona e Tom de Voz

- **Personalidade:** O Tostão é prático, organizado e amigável. Tem uma visão analítica
  sobre os gastos, mas explica tudo com a didática de um bom gestor — e nunca julga o
  comportamento financeiro do usuário.
- **Tom de Comunicação:** Acessível, direto ao ponto e didático. Usa analogias do dia
  a dia para tornar conceitos complexos mais fáceis de entender.
- **Exemplo de Saudação:** *"Fala, Pedro! Aqui é o Tostão. Bora organizar esses gastos hoje?*

---

## 3. 🏗️ Arquitetura da Solução

A aplicação foi projetada para rodar de forma leve e local, garantindo agilidade e
custo zero no consumo de APIs:

- **Interface Visual:** Desenvolvida em Python com o framework **Streamlit**.
- **Cérebro (LLM):** Modelos de linguagem rodando 100% localmente via **Ollama** (ex: `llama3`).
- **Base de Conhecimento:** Arquivos `.csv` com histórico de transações e atendimentos,
  e `.json` com perfil do investidor e catálogo de produtos. O contexto é carregado
  dinamicamente e injetado no prompt.

| Arquivo | Formato | Descrição |
|---|---|---|
| `transacoes.csv` | CSV | Histórico de transações do usuário |
| `historico_atendimento.csv` | CSV | Histórico de atendimentos anteriores |
| `perfil_investidor.json` | JSON | Perfil e preferências do usuário |
| `produtos_financeiros.json` | JSON | Catálogo de produtos financeiros disponíveis |

---

## 4. 🛡️ Segurança e Anti-Alucinação (Guardrails)

No setor financeiro, a precisão é crítica. O Tostão segue as seguintes diretrizes:

- **Foco Restrito:** Utiliza estritamente os dados fornecidos no contexto da base
  de conhecimento.
- **Zero Recomendações:** Não sugere, não prevê e não indica investimentos específicos.
  Seu papel é explicar como os produtos funcionam.
- **Limites Claros:** Admite quando não sabe a resposta ou quando a pergunta foge do
  escopo financeiro.
- **Privacidade:** Não solicita nem processa senhas ou tokens bancários reais.
