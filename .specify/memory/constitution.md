<!--
  Sync Impact Report — CopaFig Constitution v1.1.0

  Version change: 1.0.0 → 1.1.0 (MINOR — new principle + materially expanded
    guidance on business rules, color palette, and personas)

  Modified principles:
    - I. Visão do Produto — refined with country-code grid, +/- controls,
      clipboard compilation
    - II. Redução de Fricção — added clipboard-specific rule
    - III. Usabilidade Mobile — added +/- button ergonomics
    - V. Clareza Visual — added color palette reference

  Added sections:
    - VI. Personas Alvo (Casual Collector + Hardcore Collector)
    - Paleta de Cores Institucional (subsection under Restrições de Design)
    - Especificação do Grid por Países (subsection under Restrições de Design)

  Removed sections: None
  Templates requiring updates:
    - .specify/templates/plan-template.md ✅ no change needed (generic gates)
    - .specify/templates/spec-template.md ✅ no change needed
    - .specify/templates/tasks-template.md ✅ no change needed
  Deferred TODOs: Nenhum.
-->

# CopaFig Constitution
<!-- Constituição do projeto CopaFig — sistema web para troca de figurinhas do álbum da Copa do Mundo -->

## Core Principles

### I. Visão do Produto — Product Vision

CopaFig é um sistema web que elimina a fricção na troca de figurinhas do álbum da Copa do Mundo.
O usuário interage com uma grade de figurinhas organizada por siglas de países (FWC, BRA, ARG, etc.)
e, para cada figurinha, pode marcá-la como "possuída" e controlar a quantidade de repetidas via
botões de incremento (+/-). O sistema compila esses dados em uma área de transferência de texto
formatada, pronta para colar no WhatsApp — listando figurinhas faltantes, possuídas e repetidas
com suas respectivas quantidades. O valor central é reduzir o tempo entre "organizar o álbum" e
"combinar trocas" a poucos segundos, sem nenhuma barreira técnica para o usuário.

### II. Redução de Fricção na Troca — Frictionless Trading

- O fluxo principal — marcar figurinhas + ajustar quantidades → gerar relatório → copiar para área
  de transferência — DEVE ser concluível em no máximo 3 toques após o cadastro inicial.
- O relatório gerado DEVE estar formatado e pronto para colar no WhatsApp, sem necessidade de
  edição manual.
- A mensagem gerada DEVE conter três seções claras: (a) figurinhas faltantes, (b) figurinhas
  possuídas sem repetidas, (c) figurinhas repetidas com quantidades — cada seção organizada por
  código do país + número.
- Códigos das figurinhas MUST seguir as siglas oficiais dos países (FWC, BRA, ARG, etc.) com
  numeração sequencial do álbum.
- O texto copiado para a área de transferência DEVE ser texto puro (sem HTML/markdown) para
  compatibilidade universal com WhatsApp e outros mensageiros.

### III. Usabilidade Mobile e IHC — Mobile Usability & HCI

- A interface DEVE ser funcional em viewports mobile a partir de 320px de largura.
- Alvos de toque (touch targets) DEVEM ter no mínimo 44px, conforme HIG das plataformas.
- Botões de incremento (+/-) DEVEM ser grandes o suficiente para operação com o polegar em
  uma só mão, posicionados em zona de fácil alcance (terço inferior da tela).
- O grid de figurinhas DEVE usar as siglas oficiais dos países como cabeçalhos de seção,
  facilitando a navegação visual.
- Codificação visual por estados: verde = possuída sem repetidas, amarelo = possui repetidas
  (excedente), cinza = faltante, sem texto — reconhecimento instantâneo.

### IV. MVP e Entrega Incremental — MVP & Incremental Delivery

- O MVP consiste exatamente em duas páginas: Home (apresentação do projeto) + Task page
  (grade de figurinhas, marcação de estado, controle de quantidade e geração de relatório).
- Sem backend, sem persistência remota, sem cadastro de usuários no MVP — todo o estado
  e a lógica de compilação do relatório são gerenciados no client-side (JavaScript puro).
- A página de tarefas DEVE ser uma única página HTML com toda a funcionalidade auto-contida
  (sem requisições externas).
- O escopo DEVE ser expandido apenas após validação do MVP com usuários reais.

### V. Clareza Visual e Acessibilidade — Visual Clarity & Accessibility

- Contrastes de cor DEVEM atender WCAG AA: mínimo 4.5:1 para texto e 3:1 para elementos
  não-texto.
- A numeração e as siglas das figurinhas DEVEM ser legíveis (tamanho mínimo 12px) e seguir
  a ordem oficial do álbum, agrupadas por país.
- O relatório gerado DEVE ser texto puro — sem formatação HTML/markdown ou caracteres especiais
  que possam quebrar a exibição no WhatsApp.
- A paleta de cores institucional (vinho/bordeaux, amarelo ouro, tons de areia) DEVE ser usada
  como identidade visual, mas os estados das figurinhas DEVEM manter o código universal de
  semáforo (verde/amarelo/cinza) para reconhecimento imediato.

### VI. Personas Alvo — Target Personas

Todas as decisões de design DEVEM ser validadas contra estas duas personas:

**Colecionador Casual**: usuário que compra alguns pacotes por semana e completa o álbum por
diversão. Precisa de uma interface simples e intuitiva — marcar figurinhas rapidamente e
compartilhar a lista sem criar conta ou aprender um sistema complexo. Valoriza velocidade e
zero atrito.

**Colecionador Hardcore**: usuário que compra muitos pacotes, acumula dezenas de repetidas e
busca eficiência máxima nas trocas. Precisa de controle granular de quantidades (+/-),
agrupamento por países no relatório, e visibilidade clara do que falta versus o que sobra.
Valoriza precisão e relatórios detalhados.

## Restrições de Design — Design Constraints

- **Tecnologia**: HTML + CSS + JavaScript puros, sem frameworks ou bibliotecas externas
  (requisito da disciplina Programação Visual).
- **Dependências**: Nenhuma dependência npm ou ferramenta de build no MVP.
- **Arquitetura**: Página única (SPA) para a view de tarefas, com estado gerenciado via
  objeto JavaScript.

- **Modelo de dados do grid**: Mapeamento `{ [codigo]: { owned: boolean, duplicates: number } }`,
  onde `codigo` segue o padrão `SIGLA-NUMERO` (ex: `FWC-1`, `BRA-5`, `ARG-12`). A grade é
  organizada em seções por sigla de país, em ordem alfabética, com a seção especial FWC
  (FIFA World Cup) no topo.

- **Layout do grid**:
  - Seções encabeçadas pela bandeira/sigla do país
  - Cada figurinha exibe: número, checkbox/circle de estado (verde/amarelo/cinza),
    botões `-` e `+` com indicador numérico de quantidade de repetidas
  - Botões `-` e `+` desabilitados quando owned = false (figurinha faltante)
  - 5-10 colunas responsivas, adaptando-se ao viewport

- **Formato do relatório gerado** (exemplo):
  ```
  🏆 FALTANTES:
  FWC-1, FWC-3, FWC-5
  BRA-2, BRA-4, BRA-7
  ARG-1, ARG-6

  🏆 REPETIDAS:
  FWC-2 (x3), FWC-4 (x2)
  BRA-1 (x5)
  ```

- **Compartilhamento**: Botão "Copiar para área de transferência" usando a API
  `navigator.clipboard.writeText()` — sem dependência de API de sharing nativa.

### Paleta de Cores Institucional

Identidade visual inspirada nas cores da Copa do Mundo:

| Função | Cor | Hex | Aplicação |
|--------|-----|-----|-----------|
| Primária | Vinho/Bordeaux | `#8B1A2B` | Header, títulos, botão primário, footer |
| Secundária | Amarelo Ouro | `#D4A017` | Acentos, badges, destaque de ações, hover states |
| Fundo 1 | Areia Claro | `#F5E6D3` | Background geral da página |
| Fundo 2 | Areia Médio | `#E8D5C4` | Cards de figurinhas, seções de país |
| Borda | Areia Escuro | `#C4A882` | Bordas, divisores, separadores |
| Texto | Marrom Escuro | `#2C1810` | Corpo de texto, títulos secundários |
| Sucesso | Verde | `#2E7D32` | Estado "possuída sem repetidas" |
| Atenção | Amarelo | `#F9A825` | Estado "possui repetidas" |
| Inativo | Cinza | `#9E9E9E` | Estado "faltante" |

## Fluxo de Desenvolvimento — Development Workflow

- **Prototipação visual primeiro**: mockup/Figma → implementação em código.
- **Validação com conteúdo real**: testar com a lista oficial de figurinhas do álbum da Copa.
- **Teste de saída**: o relatório gerado DEVE ser copiado e colado no WhatsApp para validação
  manual do formato.
- **Iterações**: cada ciclo de feedback deve resultar em melhoria mensurável no tempo
  "intent to share".

## Governance

- Esta constituição substitui todas as práticas anteriores.
- Alterações na constituição requerem: documentação da mudança, aprovação do time e
  atualização de versão.
- Todas as decisões de design devem ser rastreáveis a necessidades do usuário com foco
  em redução de fricção e usabilidade.
- A cada alteração, os templates (plan, spec, tasks) devem ser revisados para alinhamento
  com os princípios atualizados.

**Version**: 1.1.0 | **Ratified**: 2026-06-30 | **Last Amended**: 2026-06-30
