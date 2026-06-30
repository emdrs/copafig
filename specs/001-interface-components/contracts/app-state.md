# Application State Contract

## State Shape

```typescript
interface StickerState {
  owned: boolean;
  repeats: number;
}

interface AppState {
  stickers: Record<string, StickerState>;  // key: "BRA-5"
  activeFilter: 'todas' | 'faltantes' | 'possuidas' | 'repetidas';
}
```

## State Transitions

| Action | Precondition | Mutation |
|--------|-------------|----------|
| `toggle(code)` | Nenhuma | owned = !owned; se owned passa a false, repeats = 0 |
| `increment(code)` | owned === true | repeats += 1 |
| `decrement(code)` | owned === true && repeats > 0 | repeats -= 1 |
| `setFilter(filter)` | Nenhuma | activeFilter = filter |

## Invariants

1. `owned === false` ⇒ `repeats === 0`
2. `repeats` é sempre inteiro >= 0
3. Todo código de sticker existe no estado inicial com `{ owned: false, repeats: 0 }`

## Filter Contract

| Filter | Shows |
|--------|-------|
| `todas` | Todas as figurinhas |
| `faltantes` | owned === false |
| `possuidas` | owned === true && repeats === 0 |
| `repetidas` | owned === true && repeats >= 1 |

## Report Contract

### Formato de saída (texto puro para WhatsApp)

```
🏆 *FALTANTES*
FWC-1, FWC-3, FWC-7
BRA-2, BRA-5

🏆 *POSSUÍDAS*
FWC-2, FWC-4
BRA-1

🏆 *REPETIDAS*
FWC-5 (x3)
ARG-1 (x2), ARG-3 (x1)
```

### Regras de formatação

- Seções separadas por linha em branco
- Cabeçalhos com emoji 🏆 + asteriscos *texto*
- Códigos separados por vírgula e espaço na mesma linha do país
- Repetidas com quantidade entre parênteses: `CODIGO (xN)`
- Se uma seção estiver vazia, ela DEVE ser omitida
- Ordem: FALTANTES, POSSUÍDAS, REPETIDAS
- Dentro de cada seção, ordem alfabética por sigla de país
