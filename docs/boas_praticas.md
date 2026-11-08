# Boas Práticas de Engenharia de Prompt Aplicadas

## 1. Definição explícita de papel e tarefa
**Onde:** seções `# PAPEL` e `# TAREFA`.
**O que é:** atribuir ao modelo uma função específica ("classificador de notícias
imparcial") e descrever a tarefa com verbo de ação e restrição clara ("EXATAMENTE
UMA categoria").
**Justificativa:** o papel ancora o tom e o comportamento esperado (objetivo,
neutro), e a restrição de uma única categoria elimina respostas múltiplas ou
evasivas. Instruções vagas produzem saídas inconsistentes entre execuções.

## 2. Few-shot prompting com casos de fronteira
**Onde:** seção `# EXEMPLOS` (7 pares entrada → saída).
**O que é:** fornecer exemplos resolvidos para o modelo inferir o padrão.
**Justificativa:** os exemplos cobrem todas as categorias e incluem casos
ambíguos de propósito (lei de saúde no Congresso; queda de ações de empresa
de chips). Exemplos só "fáceis" não ensinam o modelo a decidir nas zonas
cinzentas, onde os erros realmente acontecem.

## 3. Definições operacionais e regras de desempate
**Onde:** seções `# CATEGORIAS` e `# CONTEXTO E REGRAS DE DESEMPATE`.
**O que é:** descrever o escopo de cada categoria e criar critérios objetivos
para conflitos (priorizar o TEMA, não o agente ou o local).
**Justificativa:** rótulos como "Política" e "Economia" se sobrepõem no
noticiário. Sem critério explícito, o modelo decide por associação de palavras
("Congresso" → Política), gerando classificações inconsistentes.

## 4. Formato de saída estruturado e fechado
**Onde:** seção `# FORMATO DE SAÍDA` (JSON com valores enumerados).
**O que é:** exigir uma estrutura fixa, com valores permitidos predefinidos.
**Justificativa:** um JSON pode ser lido e validado automaticamente em um
pipeline (por exemplo, rejeitar categorias fora da lista). Texto livre exige
pós-processamento frágil. O campo `justificativa` também dá transparência
à decisão.

## 5. Delimitadores para separar instrução de dado
**Onde:** tags `<noticia></noticia>`.
**O que é:** isolar o conteúdo a ser classificado das instruções do prompt.
**Justificativa:** evita que o modelo confunda o texto da notícia com comandos
e reduz o risco de prompt injection (uma notícia contendo "ignore as instruções
anteriores" é tratada como dado, não como ordem).

## 6. Classe de escape e nível de confiança
**Onde:** categoria `Indefinido` e campo `confianca`.
**O que é:** permitir que o modelo declare incerteza em vez de forçar uma resposta.
**Justificativa:** sem uma saída de escape, o modelo "chuta" uma categoria para
textos fora do escopo. A confiança permite encaminhar casos `baixa` para revisão
humana, reduzindo o impacto de erros em produção.
