# Automação de lotes de integração

Aplicação em Python que transforma uma planilha de entrada em cinco arquivos padronizados de integração: CALCP, HC30%, HCP, CALCS e HSP.

A ferramenta reduz o trabalho manual de localizar colunas, reorganizar dados e preencher modelos separados.

## Funcionalidades

- Detecção automática da linha de cabeçalho
- Normalização de acentos, espaços e capitalização
- Correspondência aproximada para nomes de colunas
- Geração de cinco planilhas a partir de um único arquivo
- Preenchimento de data, resultado, solicitante e evento
- Interface desktop com Tkinter
- Empacotamento como executável com PyInstaller

## Tecnologias

- Python 3.12+
- pandas
- NumPy
- OpenPyXL
- Tkinter
- uv
- PyInstaller

## Instalação

Com o `uv` instalado:

```bash
git clone https://github.com/ICE3BR/Automacao_lotes.git
cd Automacao_lotes
uv sync
```

## Execução

```bash
uv run python main.py
```

O arquivo de modelo `testesLotes.xlsx` deve permanecer disponível para a aplicação.

## Fluxo de uso

1. Selecione a planilha de lote.
2. Confirme ou ajuste os nomes das colunas.
3. Informe o solicitante.
4. Gere os arquivos.
5. Revise as planilhas produzidas na pasta do arquivo de entrada.

## Observações

A correspondência aproximada ajuda a lidar com variações de cabeçalho, mas os arquivos gerados devem ser revisados antes de qualquer importação em sistemas externos.

Uso interno - Equipe Interna.
