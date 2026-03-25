# Base de Conhecimento

## Dados Utilizados

| Arquivo | Formato | Utilização no Agente |
| :--- | :--- | :--- |
| `historico_atendimento.csv` | CSV | Prover memória de curto prazo (log das últimas interações). |
| `perfil_cliente.json` | JSON | Direcionar o tom e os objetivos (foco na Reserva de Emergência). |
| `produtos_financeiros.json` | JSON | Servir como catálogo de referência para explicar investimentos de forma segura. |
| `transacoes.csv` | CSV | Fornecer a base quantitativa (fluxo de caixa real) para as análises do agente. |

---

## Adaptações nos Dados

Os dados mockados originais foram expandidos para refletir uma arquitetura de banco de dados relacional. O histórico de atendimento foi enriquecido com interações mais detalhadas, permitindo ao agente atuar de forma mais consultiva. O catálogo de produtos foi ajustado para manter um tom claro e objetivo, com foco em ativos de renda fixa e FIIs.

---

## Estratégia de Integração

### Como os dados são carregados?

Os dados são lidos localmente na inicialização da aplicação Streamlit. Arquivos JSON são decodificados como dicionários Python, e os arquivos CSV são processados com a biblioteca Pandas para garantir que números e categorias estejam corretamente formatados antes da injeção no prompt.

### Como os dados são usados no prompt?

Os dados são injetados dinamicamente no **System Prompt** por meio da técnica de *In-Context Learning*. O perfil do cliente, o catálogo de produtos, as transações e o histórico são concatenados nas instruções do agente antes de cada nova interação, eliminando a necessidade de um sistema de RAG com banco de dados vetorial.

---

## Exemplo de Contexto Montado

```text
Você é o Tostão, um educador financeiro consultivo e prático.
REGRAS DE SEGURANÇA: Você não realiza previsões do mercado financeiro...

--- DADOS DO USUÁRIO ---
Perfil: {'nome': 'Pedro', 'perfil_investidor': 'Moderado', 'objetivo_principal': 'Reserva de Emergência'}

Transações:
2026-03-01, Supermercado, 450.0, Compra do mês
2026-03-10, Moradia, 1200.0, Aluguel

--- HISTÓRICO DE CONVERSAS ANTERIORES ---
usuario: Fala, Tostão! Entendo de gestão de empresas por causa da minha formação em Administração na UDF, mas nas minhas finanças pessoais eu me perco um pouco.
```
