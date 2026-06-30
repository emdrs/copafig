# Research: Interface Components — Single-File UI

## Technical Decisions

### Decision: Zero-Dependency Single-File Architecture

**Rationale**: Requisito da disciplina Programação Visual e da constituição (Princípio IV).
Arquivo único HTML com CSS/JS embutido elimina necessidade de servidor, build ou instalação.

**Alternatives considered**:
- Múltiplos arquivos (HTML+CSS+JS separados) — rejeitado por complexidade de deploy e
  por violar Princípio IV (MVP auto-contido)
- Framework JS (React, Vue) — rejeitado por violar restrição de zero dependências externas
- Node.js/npm — rejeitado por violar restrição de zero build steps

### Decision: CSS Grid para Layout da Grade

**Rationale**: CSS Grid oferece responsividade nativa com `auto-fill`/`auto-fit` e `minmax()`,
permitindo que os cards quebrem em linhas automaticamente conforme o viewport.

**Alternatives considered**:
- Flexbox — viável mas requer media queries manuais para quebra de linha consistente
- Table layout — rejeitado por baixa adaptabilidade mobile
- JavaScript grid — rejeitado por violar separação de concerns e adicionar complexidade
  desnecessária

### Decision: Estado Gerenciado por Objeto JavaScript com Notificação Manual

**Rationale**: Sem frameworks nem bibliotecas reativas, o estado é um objeto plano
`{ [codigo]: { owned, repeats } }`. Uma função `render()` central lê o estado e
atualiza o DOM, chamada após cada mutação. Simples, previsível e sem dependências.

**Alternatives considered**:
- Proxy para reatividade — rejeitado por adicionar complexidade sem ganho significativo
- localStorage para persistência — rejeitado por Princípio IV (MVP sem persistência)
- URL hash state — rejeitado por complexidade desnecessária para MVP

### Decision: Relatório em Texto Puro com Formatação WhatsApp

**Rationale**: WhatsApp aceita texto puro com quebras de linha (\n), asteriscos para
negrito (*texto*), e emojis. O relatório usa estes recursos para formatação limpa
sem depender de HTML/markdown.

**Alternatives considered**:
- HTML formatado — rejeitado (WhatsApp não renderiza HTML)
- Markdown completo — rejeitado (WhatsApp não suporta todos os elementos markdown)
- JSON — rejeitado (ilegível para o usuário final)

### Decision: Paleta de Cores Constitucional

**Rationale**: Conforme Princípio V e seção "Paleta de Cores Institucional" da
constituição:
- Primária: Vinho `#8B1A2B`
- Secundária: Ouro `#D4A017`
- Fundos: Areia `#F5E6D3` / `#E8D5C4`
- Estados: Verde `#2E7D32`, Amarelo `#F9A825`, Cinza `#9E9E9E`
- Texto: Marrom `#2C1810`
