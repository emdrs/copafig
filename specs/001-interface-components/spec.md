# Feature Specification: Interface Components — Single-File UI

**Feature Branch**: `001-interface-components`

**Created**: 2026-06-30

**Status**: Draft

**Input**: User description: "Especificação detalhada dos componentes de interface para arquivo único HTML+CSS+JS"

## User Scenarios & Testing *(mandatory)*

### User Story 1 - Mark Stickers as Owned and Adjust Duplicates (Priority: P1)

Um colecionador acessa a página, visualiza a grade de figurinhas organizada por países, clica em uma figurinha para marcá-la como "possuída" e usa os botões +/- para indicar quantas repetidas possui.

**Why this priority**: Esta é a interação central do sistema — sem marcar figurinhas e controlar quantidades, não é possível gerar relatório de troca.

**Independent Test**: O usuário pode entrar na página, clicar em uma figurinha para marcá-la como possuída, ver o feedback visual imediato, e ajustar a quantidade de repetidas com +/-.

**Acceptance Scenarios**:

1. **Given** a grade de figurinhas carregada, **When** o usuário clica em uma figurinha com fundo neutro, **Then** ela muda para fundo destacado (possuída) e os botões +/- são habilitados.
2. **Given** uma figurinha marcada como possuída com 0 repetidas, **When** o usuário clica no botão +, **Then** o contador de repetidas aumenta para 1.
3. **Given** uma figurinha com 3 repetidas, **When** o usuário clica no botão -, **Then** o contador diminui para 2.
4. **Given** uma figurinha com 1 repetida, **When** o usuário clica no botão - e o contador chega a 0, **Then** a figurinha permanece como possuída (repetidas = 0).
5. **Given** uma figurinha marcada como possuída, **When** o usuário clica novamente nela, **Then** ela volta para não possuída e o contador de repetidas é zerado.

---

### User Story 2 - Filter and Navigate the Sticker Grid (Priority: P2)

O usuário usa a barra de abas (tabs) para filtrar a grade e visualizar apenas as figurinhas que lhe interessam em cada momento — todas, apenas faltantes, apenas possuídas ou apenas repetidas.

**Why this priority**: Filtrar a grade reduz a carga cognitiva e acelera a navegação, essencial para colecionadores com muitas figurinhas.

**Independent Test**: O usuário pode alternar entre abas e ver a grade se atualizar instantaneamente, mostrando apenas os cards do filtro selecionado.

**Acceptance Scenarios**:

1. **Given** a grade com figurinhas em diferentes estados, **When** o usuário clica na aba "Faltantes", **Then** apenas figurinhas com estado não possuído são exibidas.
2. **Given** o filtro "Repetidas" ativo, **When** o usuário clica na aba "Todas", **Then** todas as figurinhas são exibidas novamente.
3. **Given** nenhuma figurinha corresponde ao filtro ativo, **When** o filtro é aplicado, **Then** a grade exibe uma mensagem indicando que não há figurinhas naquele estado.

---

### User Story 3 - Generate and Copy Trading Report (Priority: P3)

Após marcar suas figurinhas, o usuário visualiza o relatório gerado automaticamente no rodapé e copia o texto formatado para a área de transferência, pronto para colar no WhatsApp.

**Why this priority**: O relatório é o produto final que viabiliza a troca — sem ele, marcar figurinhas não gera valor prático.

**Independent Test**: O usuário pode ver o relatório atualizar em tempo real no rodapé enquanto marca/desmarca figurinhas e copiá-lo com um clique.

**Acceptance Scenarios**:

1. **Given** figurinhas marcadas com diferentes estados, **When** o relatório é gerado, **Then** ele contém três seções: "FALTANTES" (códigos), "POSSUÍDAS" (códigos sem repetidas), "REPETIDAS" (códigos com quantidades).
2. **Given** o relatório visível no rodapé, **When** o usuário clica em "Copiar", **Then** o texto é copiado para a área de transferência e um feedback visual de confirmação é exibido.
3. **Given** nenhuma figurinha marcada como possuída, **When** o relatório é gerado, **Then** apenas a seção "FALTANTES" é exibida com todas as figurinhas.
4. **Given** todas as figurinhas marcadas como possuídas sem repetidas, **When** o relatório é gerado, **Then** apenas a seção "POSSUÍDAS" é exibida.

---

### Edge Cases

- O que acontece quando o navegador não suporta a API `navigator.clipboard`? O sistema deve exibir uma mensagem de erro amigável e mostrar o texto selecionável manualmente.
- O que acontece quando um código de figurinha inválido é inserido no estado? O sistema deve ignorá-lo ou resetá-lo para o padrão (não possuído).
- O que acontece se o usuário tentar diminuir repetidas abaixo de 0? O botão `-` deve ser desabilitado quando repetidas = 0 (e owned = true) ou o valor não deve ir abaixo de 0.
- O que acontece se o usuário marca uma figurinha como possuída e imediatamente desmarca? O estado de repetidas deve ser zerado junto com owned = false.
- O que acontece com a ordenação dos filtros? Filtrar por "Repetidas" deve mostrar apenas figurinhas com owned = true e repeats >= 1. "Possuídas" deve mostrar owned = true com repeats = 0.

## Requirements *(mandatory)*

### Functional Requirements

- **FR-001**: O sistema DEVE exibir um Header fixo no topo da página contendo o título "CopaFig — Programação Visual" e uma breve descrição do projeto acadêmico.
- **FR-002**: O sistema DEVE exibir uma seção "Home" com instruções de uso do sistema (como marcar figurinhas, usar os botões +/- e copiar o relatório).
- **FR-003**: O sistema DEVE fornecer uma barra de abas de navegação com pelo menos 4 filtros: "Todas", "Faltantes", "Possuídas", "Repetidas".
- **FR-004**: O sistema DEVE exibir uma grade de figurinhas onde cada card contém: código da figurinha (ex: BRA-5), indicador visual de estado (possuída/não possuída), botão `-`, indicador numérico de repetidas e botão `+`.
- **FR-005**: Ao clicar em uma figurinha não possuída, o sistema DEVE marcá-la como possuída (owned: true, repeats: 0) e atualizar o visual.
- **FR-006**: Ao clicar em uma figurinha possuída, o sistema DEVE desmarcá-la (owned: false, repeats: 0) e atualizar o visual.
- **FR-007**: O botão `+` DEVE aumentar o contador de repetidas em 1, apenas se owned = true.
- **FR-008**: O botão `-` DEVE diminuir o contador de repetidas em 1, apenas se owned = true e repeats > 0.
- **FR-009**: O sistema DEVE garantir que repeats = 0 sempre que owned = false (invariante de estado).
- **FR-010**: O sistema DEVE exibir um rodapé fixo ou área no final da página contendo o relatório de troca gerado dinamicamente.
- **FR-011**: O relatório DEVE ser atualizado automaticamente sempre que o estado de qualquer figurinha mudar.
- **FR-012**: O relatório DEVE conter três seções formatadas: "FALTANTES" (figurinhas não possuídas), "POSSUÍDAS" (possuídas sem repetidas), "REPETIDAS" (possuídas com quantidade de repetidas), organizadas por código do país.
- **FR-013**: O sistema DEVE fornecer um botão "Copiar" que copia o texto do relatório para a área de transferência e exibe feedback visual de sucesso/erro.
- **FR-014**: Figurinhas não possuídas DEVEM ter fundo neutro (tom de areia claro). Figurinhas possuídas DEVEM ter fundo destacado (tom de verde ou temático).
- **FR-015**: Os cards de figurinha DEVEM quebrar em linhas automaticamente conforme o tamanho da tela (layout flexível sem larguras fixas em pixels).
- **FR-016**: A página DEVE ser funcional em viewports de 320px a 1920px sem quebra de layout.

### Key Entities *(include if feature involves data)*

- **Figurinha (Sticker)**: Representa uma figurinha do álbum. Identificada por um código único no formato `SIGLA-NUMERO` (ex: BRA-5, FWC-1). Seus atributos de estado são: owned (boolean — se o usuário possui) e repeats (número — quantidade de repetidas, sempre 0 se owned = false).
- **Estado da Aplicação (App State)**: Objeto JavaScript que mapeia códigos de figurinha para seus respectivos estados { owned, repeats }. Gerencia a consistência dos dados (invariante: repeats = 0 quando owned = false) e notifica a UI sobre mudanças para atualização do grid e do relatório.
- **Filtro (Filter)**: Enumeração que determina quais figurinhas são exibidas na grade: "todas", "faltantes", "possuídas", "repetidas". Aplica-se como função de projeção sobre o estado da aplicação.

## Success Criteria *(mandatory)*

### Measurable Outcomes

- **SC-001**: Um colecionador consegue marcar 50 figurinhas como possuídas e ajustar repetidas em menos de 2 minutos na primeira utilização, sem instruções externas.
- **SC-002**: O relatório de troca é gerado e exibido em menos de 100ms após qualquer alteração no estado das figurinhas.
- **SC-003**: 100% dos usuários testados conseguem copiar o relatório e colar no WhatsApp com sucesso em até 3 tentativas.
- **SC-004**: A grade de figurinhas mantém legibilidade e usabilidade em viewports de 320px, 768px e 1440px sem scroll horizontal.
- **SC-005**: Usuários conseguem identificar corretamente o estado de uma figurinha (possuída vs não possuída) à primeira vista em 95% dos casos, validado por teste de percepção.

## Assumptions

- A lista de figurinhas do álbum da Copa do Mundo será fornecida como um array/objeto estático embutido no código, com códigos no formato SIGLA-NUMERO e agrupados por país.
- O navegador do usuário suporta ES6+ (JavaScript moderno), CSS Grid ou Flexbox, e a API `navigator.clipboard.writeText()`.
- Navegadores que não suportam `navigator.clipboard` terão fallback com o texto selecionável manualmente (sem depender de libraries de terceiros).
- A ordem das figurinhas na grade segue a numeração oficial do álbum: seção FWC primeiro, depois países em ordem alfabética.
- A aplicação não requer persistência de dados entre sessões — todo estado é perdido ao recarregar a página.
- Nenhuma conta de usuário ou autenticação é necessária.
