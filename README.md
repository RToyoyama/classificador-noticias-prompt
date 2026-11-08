# Classificador de Notícias com Engenharia de Prompt

Prompt estruturado para que um modelo de linguagem (LLM) classifique notícias
nas categorias **Política, Economia, Saúde, Esportes e Tecnologia**, acompanhado
de boas práticas justificadas e de uma reflexão ética sobre uso responsável e LGPD.

## Estrutura

```
classificador-noticias-prompt/
├── prompt/
│   └── prompt_classificacao.md   # Prompt completo
├── docs/
│   ├── boas_praticas.md          # 6 boas práticas justificadas
│   └── reflexao_etica.md         # Reflexão ética (~255 palavras)
├── .gitignore
└── README.md
```

## Entregas

| Requisito da atividade | Onde está |
|---|---|
| Tarefa a ser executada pelo modelo | [`prompt_classificacao.md`](prompt/prompt_classificacao.md) → `# TAREFA` |
| Exemplos de entrada e saída | [`prompt_classificacao.md`](prompt/prompt_classificacao.md) → `# EXEMPLOS` |
| Contexto adicional | [`prompt_classificacao.md`](prompt/prompt_classificacao.md) → `# CATEGORIAS` e `# CONTEXTO E REGRAS DE DESEMPATE` |
| Boas práticas listadas e justificadas | [`boas_praticas.md`](docs/boas_praticas.md) |
| Reflexão ética (transparência, viés, LGPD) | [`reflexao_etica.md`](docs/reflexao_etica.md) |

## Como usar o prompt

1. Copie o conteúdo de `prompt/prompt_classificacao.md`.
2. Substitua `{{texto_da_noticia}}` pelo título ou trecho da notícia.
3. Envie ao LLM de sua preferência.

Saída esperada:

```json
{"categoria": "Saúde", "confianca": "media", "justificativa": "O tema central é uma legislação de saúde; o Congresso é apenas o meio de aprovação."}
```

## Decisões de design

- **Few-shot com casos de fronteira**: ensina o modelo a decidir em zonas ambíguas.
- **Saída em JSON fechado**: permite validação automática em pipelines.
- **Categoria `Indefinido` + nível de confiança**: evita classificações forçadas e
  permite revisão humana.
- **Instruções de privacidade no prompt**: o modelo não reproduz dados pessoais
  na justificativa, em linha com a LGPD.
