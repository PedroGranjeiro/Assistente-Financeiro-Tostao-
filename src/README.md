# Código da Aplicação

Esta pasta contém o código do Tostão.

import streamlit as st
import pandas as pd
import json
import requests

# 1. Configuração da Página
st.set_page_config(page_title="Tostão - Gestor Financeiro", page_icon="logo.png")

# --- Cabeçalho com Logo ---
col1, col2 = st.columns([1, 5])
with col1:
    st.image("logo.png", width=70)
with col2:
    st.title("Tostão - Seu Gestor de Bolso")

# 2. Carregando a Base de Conhecimento (Lendo da pasta data/)
def carregar_dados():
    dados = {}
    try:
        with open('data/perfil_cliente.json', 'r', encoding='utf-8') as f:
            dados['perfil'] = json.load(f)
    except Exception:
        dados['perfil'] = "Perfil não encontrado."

    try:
        with open('data/produtos_financeiros.json', 'r', encoding='utf-8') as f:
            dados['produtos'] = json.load(f)
    except Exception:
        dados['produtos'] = "Catálogo não encontrado."

    try:
        df_transacoes = pd.read_csv('data/transacoes.csv')
        dados['transacoes'] = df_transacoes.to_string(index=False)
    except Exception:
        dados['transacoes'] = "Nenhuma transação encontrada."

    try:
        df_historico = pd.read_csv('data/historico_atendimento.csv')
        dados['historico'] = df_historico.tail(3).to_string(index=False)
    except Exception:
        dados['historico'] = "Nenhum histórico recente."
        
    return dados

contexto = carregar_dados()

# 3. Construindo o System Prompt com as Regras de Anti-Alucinação
system_prompt = f"""
Você é o Tostão, um educador financeiro consultivo, empático e prático.

REGRAS ESTABELECIDAS DE SEGURANÇA E ANTI-ALUCINAÇÃO (GUARDRAILS):
1. Você não realiza previsões do mercado financeiro ou especulações econômicas.
2. Você nunca recomenda a compra, venda ou alocação direta em ativos específicos.
3. Se o usuário pedir recomendações de investimento, explique as opções usando APENAS o Catálogo de Produtos.
4. Jamais responda perguntas que fujam do escopo de finanças pessoais (decline educadamente).
5. Fale de forma natural, como um brasileiro conversando no dia a dia. Evite saudações exageradas, repetitivas ou robóticas.

--- DADOS DO USUÁRIO ---
Perfil: {contexto['perfil']}
Transações Recentes: 
{contexto['transacoes']}

--- CATÁLOGO DE PRODUTOS PARA CONSULTA ---
{contexto['produtos']}

--- HISTÓRICO DE CONVERSAS ANTERIORES ---
{contexto['historico']}
"""

# 4. Função para conectar com o Ollama local
def perguntar_ao_tostao(pergunta_usuario):
    url = "http://localhost:11434/api/generate"
    prompt_completo = f"{system_prompt}\n\nNova pergunta do usuário: {pergunta_usuario}"
    
    payload = {
        "model": "llama3.2:1b",
        "prompt": prompt_completo,
        "stream": False
    }
    
    try:
        response = requests.post(url, json=payload)
        response.raise_for_status()
        return response.json().get("response", "Erro ao processar a resposta.")
    except Exception as e:
        return f"Opa! Tive um problema de conexão com o meu cérebro local. Erro: {e}"

# 5. Interface Visual do Chatbot
if "mensagens" not in st.session_state:
    st.session_state.mensagens = [{"role": "assistant", "content": "Fala, Pedro! Aqui é o Tostão. Bora organizar essas contas hoje?"}]

for msg in st.session_state.mensagens:
    with st.chat_message(msg["role"]):
        st.markdown(msg["content"])

pergunta = st.chat_input("Pergunte algo ao Tostão (ex: O que é um CDB?)")

if pergunta:
    with st.chat_message("user"):
        st.markdown(pergunta)
    st.session_state.mensagens.append({"role": "user", "content": pergunta})
    
    with st.spinner("Analisando seus dados sob rígidas regras de segurança..."):
        resposta_ia = perguntar_ao_tostao(pergunta)
        
    with st.chat_message("assistant"):
        st.markdown(resposta_ia)
    st.session_state.mensagens.append({"role": "assistant", "content": resposta_ia})
