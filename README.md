# **README.md — Automação de Lotes de Integração**

## 📌 **Descrição Geral**

Este projeto é uma ferramenta desenvolvida em Python com Tkinter para automatizar a geração de múltiplas planilhas de integração (CALCP, HC30%, HCP, CALCS e HSP) a partir de um único arquivo Excel fornecido pelo usuário.

O objetivo é eliminar trabalho manual, padronizar colunas e estruturar automaticamente os arquivos de saída com base em um  **modelo padrão (`testesLotes.xlsx`)** .

A ferramenta:

* Lê um arquivo Excel de lote
* Detecta automaticamente a linha do cabeçalho
* Localiza colunas mesmo que estejam escritas de forma diferente
* Gera uma planilha de saída para cada tipo de evento
* Aplica data atual, resultado “OK”, solicitado por e nome do evento
* Cria tudo com apenas **um clique**

A interface é simples, intuitiva e funciona tanto em **modo Python** quanto em  **arquivo executável `.exe`** .

---

## 🧩 **Tecnologias Utilizadas**

* **Python 3.12+**
* **Tkinter** (interface gráfica)
* **pandas** (tratamento das planilhas)
* **numpy**
* **difflib** (fuzzy match para reconhecimento de colunas)
* **uv** (gerenciador de projeto Python)
* **PyInstaller** (para gerar o `.exe`)

---

## 📂 Estrutura do Projeto

automacao-lotes/
│
├── main.py
├── testesLotes.xlsx   → Modelo padrão
├── pyproject.toml     → Configuração do projeto uv
└── README.md

---

# ⚙️ **Instalação e Execução (uv)**

### 1. Criar ambiente uv

uv venv
uv sync

### 2. Rodar o programa

uv run python main.py

---

# 🖥️ **Gerando o Executável (.exe)**

### 1. Gerar o `.exe`

Execute:

```
uv run pyinstaller --onefile --windowed --icon "lote.ico" --add-data "testesLotes.xlsx;." --name GeradorDeLotes main.py
```

⚠️ **Importante:**

Coloque o arquivo **testesLotes.xlsx** na mesma pasta do `.exe`.

---

# 🧠 **Como o Código Funciona (Explicação Completa)**

## ✔ 1. Carregamento do arquivo modelo

O programa busca automaticamente o modelo padrão:

<pre class="overflow-visible!" data-start="2758" data-end="2823"><div class="contain-inline-size rounded-2xl relative bg-token-sidebar-surface-primary"><div class="sticky top-9"><div class="absolute end-0 bottom-0 flex h-9 items-center pe-2"><div class="bg-token-bg-elevated-secondary text-token-text-secondary flex items-center gap-4 rounded-sm px-2 font-sans text-xs"></div></div></div><div class="overflow-y-auto p-4" dir="ltr"><code class="whitespace-pre! language-python"><span><span>ARQUIVO_MODELO = _resource_path(</span><span>"testesLotes.xlsx"</span><span>)
</span></span></code></div></div></pre>

A função `_resource_path` permite que isso funcione tanto:

* No Python normal
* Dentro do `.exe` (usando sys._MEIPASS)

---

## ✔ 2. Carregamento do arquivo em lote

A função:

<pre class="overflow-visible!" data-start="3005" data-end="3034"><div class="contain-inline-size rounded-2xl relative bg-token-sidebar-surface-primary"><div class="sticky top-9"><div class="absolute end-0 bottom-0 flex h-9 items-center pe-2"><div class="bg-token-bg-elevated-secondary text-token-text-secondary flex items-center gap-4 rounded-sm px-2 font-sans text-xs"></div></div></div><div class="overflow-y-auto p-4" dir="ltr"><code class="whitespace-pre! language-python"><span><span>carregar_lote()
</span></span></code></div></div></pre>

Faz tudo:

1. Detecta automaticamente a linha do cabeçalho
2. Localiza a coluna do processo
3. Remove colunas vazias
4. Corrige cabeçalhos bagunçados
5. Normaliza acentos e espaços

Tudo isso para evitar erros com arquivos mal formatados.

---

## ✔ 3. Normalização e reconhecimento inteligente de colunas

Função chave:

<pre class="overflow-visible!" data-start="3366" data-end="3399"><div class="contain-inline-size rounded-2xl relative bg-token-sidebar-surface-primary"><div class="sticky top-9"><div class="absolute end-0 bottom-0 flex h-9 items-center pe-2"><div class="bg-token-bg-elevated-secondary text-token-text-secondary flex items-center gap-4 rounded-sm px-2 font-sans text-xs"></div></div></div><div class="overflow-y-auto p-4" dir="ltr"><code class="whitespace-pre! language-python"><span><span>_find_best_column()
</span></span></code></div></div></pre>

Ela reconhece colunas mesmo se o nome estiver:

* Com acentos
* Com letras maiúsculas/minúsculas diferentes
* Com espaços duplos ou NBSP
* Escrito errado (aproximação fuzzy)

Exemplo:

"Numero    do   Processo"

"número do processo"

"NUMERO   DO  PROCESSO"

"num proc"

Todos viram a mesma coluna corretamente.

---

## ✔ 4. Montagem das planilhas de saída

A função:

<pre class="overflow-visible!" data-start="3783" data-end="3811"><div class="contain-inline-size rounded-2xl relative bg-token-sidebar-surface-primary"><div class="sticky top-9"><div class="absolute end-0 bottom-0 flex h-9 items-center pe-2"><div class="bg-token-bg-elevated-secondary text-token-text-secondary flex items-center gap-4 rounded-sm px-2 font-sans text-xs"></div></div></div><div class="overflow-y-auto p-4" dir="ltr"><code class="whitespace-pre! language-python"><span><span>montar_saida()
</span></span></code></div></div></pre>

Cria um DataFrame com as colunas do modelo e insere automaticamente:

| Coluna            | Conteúdo                     |
| ----------------- | ----------------------------- |
| PROCESSO          | Valor extraído do lote       |
| EVENTO            | Valor da coluna escolhida     |
| DATA              | Data atual                    |
| RESULT            | OK                            |
| SOLICITADO_POR    | Valor digitado na interface   |
| EVENTO_INTEGRACAO | Nome do evento (CALCP, HC30%) |

---

## ✔ 5. Geração automática dos 5 arquivos

Os arquivos gerados são:

1 Cópia de modelo rb 03 - CALCP preenchido.xlsx
1 Cópia de modelo rb 03 - HC30% preenchido.xlsx
1 Cópia de modelo rb 03 - HCP preenchido.xlsx
1 Cópia de modelo rb 03 - CALCS preenchido.xlsx
1 Cópia de modelo rb 03 - HSP preenchido.xlsx

Cada um é salvo na  **mesma pasta do arquivo de lote** .

---

# 🖼️ **Como Usar a Interface**

1. Abra o programa (`uv run python main.py` ou `.exe`)
2. Clique em **Selecionar...** e escolha o arquivo do lote
3. Preencha os nomes das colunas (ou deixe os padrões)
4. Informe o **SOLICITADO_POR**
5. Clique em **Gerar Arquivos**

A ferramenta:

* Lê a planilha
* Identifica automaticamente as colunas
* Gera todos os arquivos de uma vez
* Exibe mensagem de sucesso

---

# 📄 **Modelo de Entrada Obrigatório**

O arquivo **testesLotes.xlsx** deve conter ao menos:

* A coluna do número do processo
* As colunas informadas na interface para:
  * CALCP
  * HC30%
  * HCP
  * CALCS
  * HSP

---

# 📜 Licença

Uso interno — Equipe Legal Ops.

---
