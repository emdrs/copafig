# Data Model: Interface Components — Single-File UI

## Entities

### Sticker (Figurinha)

| Field | Type | Description | Constraints |
|-------|------|-------------|-------------|
| `code` | `string` | Identificador único no formato `SIGLA-NUMERO` (ex: `BRA-5`, `FWC-1`) | Formato: `/^[A-Z]{3}-\d+$/`, único |
| `owned` | `boolean` | Se o usuário possui esta figurinha | `false` por padrão |
| `repeats` | `number` | Quantidade de repetidas | `0` quando `owned = false`; `>= 0` quando `owned = true` |

**Invariants**:
1. Se `owned === false`, então `repeats === 0` (não podem existir repetidas de uma
   figurinha que não se possui)
2. Se `owned === true`, então `repeats >= 0`

**State Transitions**:
- `click` em figurinha não possuída: `owned: false → true`, `repeats: 0`
- `click` em figurinha possuída: `owned: true → false`, `repeats: 0`
- `+` click: `repeats += 1` (apenas se `owned === true`)
- `-` click: `repeats -= 1` (apenas se `owned === true` e `repeats > 0`)

### Country (País)

| Field | Type | Description |
|-------|------|-------------|
| `code` | `string` | Sigla de 3 letras (FWC, BRA, ARG...) |
| `name` | `string` | Nome do país por extenso |
| `stickers` | `Sticker[]` | Lista de figurinhas deste país |

**Ordering**: FWC (FIFA World Cup) sempre primeiro, depois países em ordem alfabética.

### AppState

| Field | Type | Description |
|-------|------|-------------|
| `stickers` | `Map<string, Sticker>` | Mapa código → sticker |
| `activeFilter` | `'todas' | 'faltantes' | 'possuidas' | 'repetidas'` | Filtro ativo da grade |

**Behavior**: Centraliza todas as mutações e garante invariantes. Após cada mutação,
dispara `render()` para atualizar DOM.

## Data Flow

```
User click → AppState.toggle(code)          → render()
User click + → AppState.increment(code)      → render()
User click - → AppState.decrement(code)      → render()
Filter tab  → AppState.setFilter(filter)     → render()
             → AppState.getReport()          → render() (footer)
```
