# Implementation Plan: Interface Components — Single-File UI

**Branch**: `001-interface-components` | **Date**: 2026-06-30 | **Spec**: specs/001-interface-components/spec.md

**Input**: Feature specification from `specs/001-interface-components/spec.md`

**Note**: This template is filled in by the `/speckit.plan` command.

## Summary

Single HTML file containing the complete CopaFig sticker album trading interface. Users
interact with a country-organized sticker grid (FWC, BRA, ARG...), mark stickers as owned,
control duplicate counts via +/- buttons, filter by status tabs, and copy a formatted
WhatsApp-ready trading report.

## Technical Context

**Language/Version**: HTML5 + CSS3 + JavaScript ES6+ (no transpilation)

**Primary Dependencies**: Nenhuma — zero external dependencies, zero npm packages.
Arquivo único auto-contido.

**Storage**: Client-side apenas — estado gerenciado em memória (objeto JavaScript).
Sem localStorage, cookies ou persistência.

**Testing**: Validação manual via inspeção visual e teste de copia-e-cola no WhatsApp.

**Target Platform**: Navegadores modernos (Chrome 80+, Firefox 80+, Safari 14+, Edge 80+)
com suporte a CSS Grid/Flexbox e `navigator.clipboard`.

**Project Type**: Static single-page web application (single .html file)

**Performance Goals**: Relatório regenerado em <50ms após qualquer mudança de estado.
Grid com scroll suave mesmo com ~600 figurinhas.

**Constraints**: Zero dependências externas. Zero build steps. Zero frameworks.
Arquivo único funcional ao abrir no navegador. Touch targets >= 44px.
Contraste WCAG AA. Responsivo 320px-1920px.

**Scale/Scope**: Página única, ~200-600 figurinhas do álbum, único usuário local.

## Constitution Check

*GATE: Must pass before Phase 0 research. Re-check after Phase 1 design.*

- **I. Visão do Produto**: ✅ Grade por siglas de países, owned/repeats, clipboard.
- **II. Redução de Fricção**: ✅ 3 seções no relatório, texto puro, cópia com 1 clique.
- **III. Usabilidade Mobile**: ✅ Viewport 320px+, touch 44px, +/- ergonômicos, grid responsivo.
- **IV. MVP Incremental**: ✅ Arquivo único, sem backend, sem persistência, client-side.
- **V. Clareza Visual**: ✅ Paleta vinho/ouro/areia + verde/amarelo/cinza para estados.
- **VI. Personas**: ✅ Casual (intuitivo, poucos cliques) + Hardcore (controle granular, +/-).

**Resultado**: GATE PASSED — nenhuma violação. Nenhuma justificativa de complexidade necessária.

## Project Structure

### Documentation (this feature)

```text
specs/001-interface-components/
├── plan.md              # This file
├── research.md          # Phase 0 output
├── data-model.md        # Phase 1 output
├── quickstart.md        # Phase 1 output
├── contracts/           # Phase 1 output
└── tasks.md             # Phase 2 output (NOT created by /speckit.plan)
```

### Source Code (repository root)

```text
index.html               # Single file — complete application
```

**Structure Decision**: Single file — `index.html` na raiz do repositório. Todo o CSS
embutido em `<style>`, todo o JS em `<script>`. Sem diretórios adicionais.

## Complexity Tracking

Nenhuma violação de Constitution Check — seção não aplicável.
