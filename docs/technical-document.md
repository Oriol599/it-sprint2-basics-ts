# Document Tècnic: TypeScript Basics Refactor & Transformation Types

## Visió General del Projecte

Aquest repositori implementa una sèrie d'exercicis progressius per aprendre i practicar conceptes fonamentals i avançats de **TypeScript**. Està estructurat en dos mòduls principals que cobreixen des de la migració de JavaScript a TypeScript fins a l'ús avançat d'*Utility Types* i transformació de tipus.

---

## Estructura del Projecte

```
src/
├── basics-refactor.test.ts      # Mòdul 1: 18 reptes (Basics Refactor)
├── transformation-types.test.ts # Mòdul 2: 13 reptes (Transformation Types)
└── helpers/
    └── type-utils.ts            # Utilitats de tipus reutilitzables
```

---

## Mòdul 1: Basics Refactor (18 Reptes)

### Objectiu
Migració progressiva de JavaScript a TypeScript, resolvent errors de tipus pas a pas seguint la [documentació oficial de migració](https://www.typescriptlang.org/docs/handbook/migrating-from-javascript.html).

---

### Reptes 1-4: Tipatge Bàsic de Funcions i Paràmetres

| Repte | Concepte Clau | Solució |
|-------|---------------|---------|
| **1** | Tipatge de paràmetres simples | `const addTwoNumbers = (a: number, b: number) => a + b` |
| **2** | Tipatge d'objecte com a paràmetre | `const addTwoNumbers = (params: { first: number; second: number }) => params.first + params.second` |
| **3** | Propietats opcionals en objectes | `{ first: string; last?: string }` - operador `?` |
| **4** | Paràmetres opcionals en funcions | `const getName = (first: string, last?: string) => ...` |

**Concepte clau**: La diferència entre `?` en propietats d'objecte vs paràmetres de funció.

---

### Reptes 5-7: Interfícies, Unions i Arrays

| Repte | Concepte Clau | Solució |
|-------|---------------|---------|
| **5** | Assignació de tipus a variables | `const defaultUser: User = { ... }` - anotació explícita en la declaració |
| **6** | Union Types (Literal Types) | `role: "admin" \| "user" \| "super-admin"` - restringeix valors literals |
| **7** | Tipatge d'arrays d'objectes | `posts: Post[]` o `posts: Array<Post>` |

**Concepte clau**: Els *Literal Types* (`"admin" | "user"`) restringeixen els valors vàlids en temps de compilació.

---

### Reptes 8-10: Funcions, Promeses i Sets

| Repte | Concepte Clau | Solució |
|-------|---------------|---------|
| **8** | Return Type Annotation | `const makeUser = (): User => { ... }` - anota el tipus de retorn |
| **9** | Async/Await i Promises | `const fetchLukeSkywalker = async (): Promise<LukeSkywalker> => { ... }` |
| **10** | Tipatge de `Set` | `const guitarists = new Set<string>()` |

**Concepte clau**: `Promise<T>` per funcions asíncrones; `Set<T>` per col·leccions úniques tipades.

---

### Reptes 11-13: Tipus Utilitats Avançats

| Repte | Concepte Clau | Solució |
|-------|---------------|---------|
| **11** | `Record<K, V>` | `const cache: Record<string, string> = {}` - objecte amb claus string i valors string |
| **12** | Type Guards amb `typeof` | `if (typeof amount === "object")` - narrowing de `number \| { amount: number }` |
| **13** | Error handling amb `unknown` | `catch (e: unknown)` + `e instanceof Error` type guard |

**Concepte clau**: 
- `Record<K, V>`: Tipus d'objecte amb claus `K` i valors `V`
- `unknown`: Tipus segur per `catch` (requereix type guards)
- Type Guards: `typeof`, `instanceof`, `in` per *narrowing*

---

### Reptes 14-18: Patrons Avançats i Utility Types

| Repte | Concepte Clau | Solució |
|-------|---------------|---------|
| **14** | Herència d'interfícies (`extends`) | `interface User extends Who { ... }` - reutilitza `id: string` |
| **15** | Intersection Types (`&`) | `User & { posts: Post[] }` - combina tipus |
| **16** | `Pick` / `Omit` Utility Types | `Pick<User, 'firstName' \| 'lastName'>` o `Omit<User, 'id'>` |
| **17** | Tipatge de callbacks | `onFocusChange: (isFocused: boolean) => void` |
| **18** | Funcions amb Promises com a paràmetres | `createUser: () => Promise<string>`, `getUser: (id: string) => Promise<User>` |

**Concepte clau**:
- `Pick<T, K>`: Selecciona propietats `K` de `T`
- `Omit<T, K>`: Exclou propietats `K` de `T`
- `T & U`: Intersecció (totes les propietats de tots dos)

---

### Repte Addicional: `ReturnType`

```typescript
// Extreu el tipus de retorn d'una funció
type MyFuncReturn = ReturnType<typeof myFunc>;
```

---

## Mòdul 2: Transformation Types (13 Reptes)

### Objectiu
Domini dels **Utility Types** de TypeScript per transformar, extreure i manipular tipus existents.

---

### Reptes 1-3: Function Type Extraction

| Repte | Utility Type | Aplicació |
|-------|--------------|-----------|
| **1** | `ReturnType<T>` | `ReturnType<typeof myFunc>` → `string` |
| **2** | `Parameters<T>` | `Parameters<typeof makeQuery>` → tupla de paràmetres |
| **3** | `Awaited<ReturnType<T>>` | Desenvolta `Promise<T>` → `T` |

**Patró clau**: `typeof functionName` obté el tipus de la funció, després s'aplica l'Utility Type.

---

### Reptes 4-8: Indexed Access Types (`[key]`)

| Repte | Concepte | Sintaxi |
|-------|----------|---------|
| **4** | `keyof typeof obj` | `keyof typeof testingFrameworks` → `"vitest" \| "jest" \| "mocha"` |
| **5** | `typeof obj["key"]` | `typeof fakeDataDefaults["String"]` → `string` |
| **6** | Union d'access indexat | `typeof map["A" \| "B"]` → unió de valors |
| **7** | Array access: `[number]` | `typeof fruits[number]` → unió de tots els elements |
| **8** | Object values: `[keyof typeof obj]` | `typeof obj[keyof typeof obj]` → unió de tots els valors |

**Concepte clau**: `as const` fa l'objecte/array *readonly* i infereix *literal types*.

---

### Reptes 9-12: Discriminated Unions & Utility Types

| Repte | Concepte | Utility Type / Sintaxi |
|-------|----------|------------------------|
| **9** | Terminologia | Union \| Discriminated Union \| Enum |
| **10** | `Extract<T, U>` | `Extract<Event, { type: "click" }>` → extreu branques |
| **11** | `Exclude<T, U>` | `Exclude<Event, { type: "keydown" }>` -> exclou branques |
| **12** | Discriminant extraction | `Event["type"]` → `"click" \| "focus" \| "keydown"` |

**Discriminated Union**: Union on cada branca té una propietat comuna (*discriminant*) de tipus literal.

---

### Repte 13: Indexed Access amb Arrays (Repte Final)

```typescript
const fruits = ["apple", "banana", "orange"] as const;

type AppleOrBanana = typeof fruits[0 | 1];  // "apple" | "banana"
type Fruit = typeof fruits[number];         // "apple" | "banana" | "orange"
```

---

## Mòdul Suport: `type-utils.ts` (Utilitats de Tipus)

Aquest fitxer proporciona *type utilities* reutilitzables per als tests:

| Utilitat | Propòsit |
|----------|----------|
| `Expect<T extends true>` | Assert que `T` és `true` en temps de compilació |
| `Equal<X, Y>` | Compara igualtat de tipus (exclou `any`) |
| `NotEqual<X, Y>` | Inversa de `Equal` |
| `IsAny<T>` / `NotAny<T>` | Detecta si un tipus és `any` |
| `Alike<X, Y>` | Comparació estructural flexible |
| `ExpectExtends<VALUE, EXPECTED>` | Comprova `EXPECTED extends VALUE` |
| `ExpectValidArgs<FUNC, ARGS>` | Valida arguments de funció |
| `UnionToIntersection<U>` | Converteix union a intersecció |

**Tècnica clau `Equal`**: Utilitza inferència de funcions genèriques per comparar tipus sense `any`.

---

## Resum de Conceptes Clau per Mòdul

### Basics Refactor
1. **Tipus primitives** → `number`, `string`, `boolean`
2. **Object Types** → `{ prop: Type }`, `?` opcional
3. **Function Types** → Paràmetres, Return Type, Optional params
4. **Interfaces** → Contractes reutilitzables, `extends`
5. **Union Types** → `"a" \| "b"`, Discriminated Unions
6. **Arrays/Tuples** → `Type[]`, `[Type, Type]`
7. **Generics bàsics** → `Promise<T>`, `Set<T>`, `Array<T>`
8. **Utility Types bàsics** → `Record<K,V>`, `Pick<T,K>`, `Omit<T,K>`
9. **Type Guards** → `typeof`, `instanceof`, narrowing
10. **Intersection Types** → `A & B`

### Transformation Types
1. **Function Extraction** → `ReturnType`, `Parameters`, `Awaited`
2. **Indexed Access** → `T[K]`, `keyof T`, `T[number]`
3. **`as const`** → Literal Types, Readonly
4. **Discriminated Unions** → `type` discriminant, `Extract`, `Exclude`
5. **Utility Types avançats** → Manipulació de unions

---

## Com Executar els Tests

```bash
# Instal·lar dependències
npm install

# Executar tests en mode watch
npm run test

# Executar tests una sola vegada
npm run test:run
```

Configuració: **Vitest** + **TypeScript** (tipus estricts activats)

---

## Recursos de Referència

- [TypeScript Handbook - Migrating from JavaScript](https://www.typescriptlang.org/docs/handbook/migrating-from-javascript.html)
- [TypeScript Utility Types](https://www.typescriptlang.org/docs/handbook/utility-types.html)
- [TypeScript 2.8 - Conditional Types](https://www.typescriptlang.org/docs/handbook/2/conditional-types.html)
- [TypeScript - Indexed Access Types](https://www.typescriptlang.org/docs/handbook/2/indexed-access-types.html)
- [Vitest - Type Testing](https://vitest.dev/guide/testing-types.html)