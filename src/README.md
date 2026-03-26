# 💰 Código da Aplicação

Esta pasta contém o motor principal do **Tostão**, o agente financeiro — toda a lógica 
de interface, conexão com o modelo de IA local e processamento do banco de dados simulado.

## 📁 Estrutura
```text
src/
├── app.py              # Aplicação principal (Interface Streamlit + Guardrails)
├── gerador_slides.py   # Script auxiliar para apresentações
└── README.md           # Instruções de execução local
```

## 📦 Dependências
```text
streamlit==1.32.0
pandas==2.2.1
requests==2.31.0
```

## 🚀 Como Rodar
```bash
# 1. Instale as dependências do Python
pip install -r requirements.txt

# 2. Inicie o modelo de IA em segundo plano (requer o Ollama instalado)
ollama run llama3.2:1b

# 3. Rode a interface web apontando para a pasta src
python -m streamlit run src/app.py
```
