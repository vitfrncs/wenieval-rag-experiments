# WeniEval — Recuperação de Informação e Agentic RAG

Este projeto reúne experimentos de análise de dados, recuperação de informação e agentes com LLMs, utilizando o dataset [WeniEval-Benchmark-2.0.0](https://huggingface.co/datasets/Weni/WeniEval-Benchmark-2.0.0).

A ideia foi estudar como construir uma base de conhecimento para RAG, comparar diferentes estratégias de recuperação e entender como um agente pode utilizar as informações recuperadas para responder a perguntas.

O desenvolvimento foi realizado em Jupyter Notebooks, com explicações dos conceitos, decisões de implementação e resultados dos experimentos.

## O que foi desenvolvido

O projeto está dividido em dois notebooks:

### `eda.ipynb` — Análise exploratória

O dataset possui 3.887 registros e 18 colunas, com perguntas, conteúdos e metadados em diferentes idiomas.

A análise incluiu a distribuição dos dados, valores ausentes, duplicatas, idiomas e comprimento dos textos.

Um dos pontos observados foi a reutilização de conteúdos em vários registros. Após a normalização, foram identificados 535 conteúdos distintos, utilizados posteriormente na construção da base de recuperação.

### `agente.ipynb` — Recuperação e agente

Foram implementadas duas estratégias de recuperação:

- TF-IDF, utilizando Scikit-learn;
- Embeddings semânticos com `intfloat/multilingual-e5-small`.

Os documentos foram divididos em chunks de 350 tokens, com sobreposição de 50 tokens. A recuperação é feita sobre esses trechos, preservando a identificação dos documentos de origem.

A comparação foi realizada utilizando perguntas em português, inglês e espanhol. As métricas, os resultados e a análise dos erros estão documentados no notebook.

Também desenvolvido um protótipo de agente utilizando `smolagents` e `Qwen/Qwen3-8B`. O agente possui uma ferramenta de busca semântica que permite consultar os documentos antes de gerar uma resposta.

## Observações e limitações

Durante os testes, alguns problemas ficaram evidentes.

Em determinados casos, a fonte correta era recuperada, mas o chunk selecionado não continha a informação necessária para responder à pergunta. Também foi observado um caso em que o agente misturou informações de documentos com políticas diferentes.

Esses resultados mostram que recuperar documentos relacionados à consulta não é suficiente, já que a seleção dos trechos e a identificação das fontes também afetam a resposta gerada.

Os testes com o agente também apresentaram problemas de integração com o smolagents, pois o modelo nem sempre gerou as ações no formato esperado pelo framework. Além disso, a avaliação foi interrompida pelo esgotamento dos créditos da API de inferência.


## Como executar

Crie um ambiente virtual e instale as dependências:

```bash
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

Abra os arquivos no Jupyter Notebook ou JupyterLab e execute primeiro `eda.ipynb`, seguido de `agente.ipynb`.

Para as células que utilizam o agente, é necessário configurar a variável de ambiente `HF_TOKEN` com um token do Hugging Face. A execução também depende da disponibilidade do modelo no serviço de inferência.

## Referências

- [WeniEval-Benchmark-2.0.0](https://huggingface.co/datasets/Weni/WeniEval-Benchmark-2.0.0)
- [Sentence Transformers](https://www.sbert.net/)
- [Scikit-learn — TF-IDF](https://scikit-learn.org/stable/modules/generated/sklearn.feature_extraction.text.TfidfVectorizer.html)
- [smolagents](https://huggingface.co/docs/smolagents/)