---
name: compactacao
description: Use quando o usuário digitar "compactar" para resumir toda a conversa em até 10 bullets com contexto crítico, decisões e trechos essenciais, formatados em markdown para uso em um novo chat.
---

# Compactação de Chat

## Gatilho

Quando o usuário escrever **"compactar"** (ou variações como "compacta isso", "compacta o chat").

## Escopo de contexto

**Use APENAS a conversa atual.** Não consulte memórias de sessões anteriores, não use o sistema de memória, não traga informações de outros chats. Se não houver conversa suficiente, informe o usuário.

## Comportamento

1. Varrer toda a conversa **atual** do início ao fim
2. Extrair os elementos de maior valor informacional (ver prioridade abaixo)
3. Condensar em **no máximo 10 bullets**, ordenados por relevância
4. Usar a ferramenta **Write** para criar o arquivo `contexto-chat.md` no diretório de trabalho atual (`./contexto-chat.md`)
5. Exibir o conteúdo do arquivo na resposta

## Prioridade dos bullets

Selecione os bullets mais relevantes nesta ordem de importância:

| # | O que capturar |
|---|----------------|
| 1 | Objetivo/tarefa principal da conversa |
| 2 | Decisões irreversíveis tomadas |
| 3 | Erros críticos encontrados e suas soluções |
| 4 | Arquivos-chave criados/modificados com seus caminhos |
| 5 | Estado atual do trabalho (o que foi feito, o que falta) |
| 6 | Próximos passos acordados |
| 7 | Constraints e requisitos não-óbvios |
| 8 | Snippets de código essenciais (apenas se pequenos e insubstituíveis) |
| 9 | Contexto de negócio ou produto relevante |
| 10 | Qualquer informação sem a qual o contexto seria perdido |

**Regras:**
- Cada bullet: máx. 2 linhas
- Se um snippet de código for essencial, use bloco inline curto dentro do bullet
- Omita detalhes que podem ser redescobertos lendo o código
- Priorize o que é difícil de reconstruir sem a conversa

## Formato de output

Gere o arquivo `contexto-chat.md` com este formato exato:

```markdown
# Contexto do Chat — {DATA_HOJE}

## Objetivo
{Uma frase descrevendo o que estava sendo feito}

## Contexto & Decisões
- {bullet 1}
- {bullet 2}
- {bullet 3}
...

---
*Gerado por /compactar em {DATA_HOJE}*
```

Após gerar o arquivo, exiba na resposta:

```
Arquivo salvo em: contexto-chat.md

Para continuar em um novo chat, cole este bloco no início:
---
[conteúdo do arquivo]
---
```

## Erros comuns a evitar

- Não inclua bullets óbvios ("o usuário pediu para criar uma skill") — capture o *quê* e o *porquê*, não o *que foi dito*
- Não repita informação já contida em outro bullet
- Não ultrapasse 10 bullets — se necessário, mescle bullets menos críticos
- Não inclua conversas de ida-e-volta — apenas fatos, decisões e estado
- Não exiba o conteúdo na resposta sem antes usar a ferramenta **Write** — o arquivo deve ser criado no sistema de arquivos
- Não inclua informações de sessões anteriores ou do sistema de memória — apenas o chat atual conta
