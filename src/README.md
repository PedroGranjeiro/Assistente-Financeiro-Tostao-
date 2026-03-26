# Código da Aplicação

Esta pasta contém o motor principal do **Tostão**, o agente financeiro. Aqui reside toda a lógica de interface, conexão com o modelo de inteligência artificial local e processamento do banco de dados simulado.

## Estrutura da Aplicação

A arquitetura do código foi projetada para ser minimalista, focada em segurança de dados (On-Premise) e de fácil manutenção.

```text
src/
├── app.py              # Aplicação principal (Interface Streamlit + Guardrails)
├── gerador_slides.py   # Script auxiliar para apresentações
└── README.md           # Instruções de execução local
Dependências
O projeto utiliza bibliotecas padrão de mercado para manipulação de dados e criação de interfaces web. O modelo de linguagem (LLM) roda nativamente através do Ollama, atuando como um servidor local.

Plaintext
streamlit==1.32.0
pandas==2.2.1
requests==2.31.0
Como Rodar o Agente
Siga os passos abaixo no seu terminal (certifique-se de estar na pasta raiz do projeto) para iniciar o Tostão:

Bash
# 1. Instale as dependências do Python
pip install -r requirements.txt

# 2. Inicie o "cérebro" da IA em segundo plano (requer o Ollama instalado)
ollama run llama3.2:1b

# 3. Rode a interface web apontando para a pasta src
python -m streamlit run src/app.py
