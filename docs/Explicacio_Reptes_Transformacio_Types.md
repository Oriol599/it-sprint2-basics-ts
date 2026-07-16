# Explicació dels 13 Reptes de Transformació de Tipus (transformation-types)

## Repte 1: Obtenir el tipus de retorn d'una funció
**Concepte après:** Utility Type `ReturnType<T>` combinat amb `typeof`.

```typescript
const myFunc = () => {
  return "hello";
};

type MyFuncReturn = ReturnType<typeof myFunc>;
```

`ReturnType<T>` és un **Utility Type** que extreu el tipus de retorn d'un tipus de funció. Com que `myFunc` és una variable (no un tipus), primer usem `typeof myFunc` per obtenir-ne el seu tipus, i després `ReturnType` n'extreu el retorn (`string`).

> **Nota:** `ReturnType` només funciona amb tipus de funció. Per això és necessari l'ús previ de `typeof`.

---

## Repte 2: Obtenir els paràmetres d'una funció
**Concepte après:** Utility Type `Parameters<T>`.

```typescript
const makeQuery = (
  url: string,
  opts?: {
    method?: string;
    headers?: { [key: string]: string };
    body?: string;
  },
) => {};

type MakeQueryParameters = Parameters<typeof makeQuery>;
```

`Parameters<T>` extreu els paràmetres d'una funció com una **tupla**. Cada element de la tupla correspon a un paràmetre, mantenint el seu nom i tipus. Els paràmetres opcionals (`?`) es conserven com a opcionals dins la tupla.

El resultat és: `[url: string, opts?: { method?: string; headers?: { [key: string]: string; }; body?: string; }]`

> **Nota:** TypeScript 4.0+ permet etiquetar els elements d'una tupla amb noms, com es veu a `url:` i `opts?:`.

---

## Repte 3: Obtenir el tipus de retorn d'una funció asíncrona
**Concepte après:** Utility Types `ReturnType<T>` i `Awaited<T>`.

```typescript
const getUser = () => {
  return Promise.resolve({
    id: "123",
    name: "John",
    email: "john@example.com",
  });
};

type ReturnValue = Awaited<ReturnType<typeof getUser>>;
```

Una funció asíncrona retorna una `Promise`. `ReturnType<typeof getUser>` obtindria `Promise<{ id: string; name: string; email: string }>`. Per "desembolicar" la Promise i obtenir el tipus real, usem `Awaited<T>`, que extreu el tipus intern d'una promesa.

> **Nota:** `Awaited<T>` és un Utility Type introduït a TypeScript 4.5. És especialment útil per a funcions que retornen Promeses o cadenes de Promeses anidades.

---

## Repte 4: Obtenir les claus d'un objecte com a unió
**Concepte après:** Operador `keyof` combinat amb `typeof`.

```typescript
const testingFrameworks = {
  vitest: { label: "Vitest" },
  jest: { label: "Jest" },
  mocha: { label: "Mocha" },
};

type TestingFramework = keyof typeof testingFrameworks;
```

`keyof` retorna una **unió de les claus** d'un tipus d'objecte. Com que `testingFrameworks` és una variable, usem `typeof` per obtenir-ne el seu tipus abans d'aplicar `keyof`. El resultat és `"vitest" | "jest" | "mocha"`.

> **Nota:** `keyof` és un operador de TypeScript, no una funció. Funciona en temps de compilació, no d'execució.

---

## Repte 5: Indexed access types amb objectes
**Concepte après:** Accés indexat a tipus amb `T[K]`.

```typescript
const fakeDataDefaults = {
  String: "Default string",
  Int: 1,
  Float: 1.14,
  Boolean: true,
  ID: "id",
};

type StringType = typeof fakeDataDefaults["String"];  // string
type IntType = typeof fakeDataDefaults["Int"];        // number
type FloatType = typeof fakeDataDefaults["Float"];    // number
type BooleanType = typeof fakeDataDefaults["Boolean"]; // boolean
type IDType = typeof fakeDataDefaults["ID"];          // string
```

L'accés indexat a tipus (`T[K]`) funciona de manera similar a l'accés a propietats en JavaScript, però en temps de compilació. `typeof fakeDataDefaults["String"]` obté el tipus de la propietat `String` de l'objecte, que és `string`.

> **Nota:** A diferència de JavaScript, on `obj["prop"]` retorna el valor, aquí retorna el **tipus** de la propietat. És una eina poderosa per a la inferència de tipus dinàmica.

---

## Repte 6: Indexed access amb unions
**Concepte après:** Accés indexat amb múltiples claus per obtenir una unió de tipus.

```typescript
const programModeEnumMap = {
  GROUP: "group",
  ANNOUNCEMENT: "announcement",
  ONE_ON_ONE: "1on1",
  SELF_DIRECTED: "selfDirected",
  PLANNED_ONE_ON_ONE: "planned1on1",
  PLANNED_SELF_DIRECTED: "plannedSelfDirected",
} as const;

type IndividualProgram = typeof programModeEnumMap[
  "ONE_ON_ONE" | "SELF_DIRECTED" | "PLANNED_ONE_ON_ONE" | "PLANNED_SELF_DIRECTED"
];
```

Quan s'accedeix a un objecte amb múltiples claus (una unió), TypeScript retorna una **unió dels tipus** corresponents. L'ús d'`as const` fa que els valors literals (`"1on1"`, `"selfDirected"`, etc.) siguin tractats com a tipus literals, no com a `string`.

El resultat és: `"1on1" | "selfDirected" | "planned1on1" | "plannedSelfDirected"`

> **Nota:** `as const` és fonamental aquí. Sense ell, els valors serien de tipus `string` en lloc dels literals exactes.

---

## Repte 7: Obtenir el tipus dels valors d'un array
**Concepte après:** Accés indexat a arrays amb `T[number]` i índexs específics.

```typescript
const fruits = ["apple", "banana", "orange"] as const;

type AppleOrBanana = typeof fruits[0 | 1];  // "apple" | "banana"
type Fruit = typeof fruits[number];          // "apple" | "banana" | "orange"
```

- `typeof fruits[0 | 1]`: accedeix als tipus en les posicions 0 i 1 de l'array. Com que l'array té `as const`, els valors són literals.
- `typeof fruits[number]`: és una sintaxi especial que significa "tots els índexs numèrics", retornant una unió de tots els elements de l'array.

> **Nota:** `T[number]` és equivalent a "qualsevol índex numèric" i és la forma habitual d'extreure els tipus dels valors d'un array/tupla.

---

## Repte 8: Obtenir el tipus dels valors d'un objecte amb as const
**Concepte après:** Accés indexat combinat amb `keyof` sobre objectes `as const`.

```typescript
const frontendToBackendEnumMap = {
  singleModule: "SINGLE_MODULE",
  multiModule: "MULTI_MODULE",
  sharedModule: "SHARED_MODULE",
} as const;

type BackendModuleEnum = typeof frontendToBackendEnumMap[keyof typeof frontendToBackendEnumMap];
```

Aquesta és una combinació de tècniques anteriors:
1. `typeof frontendToBackendEnumMap` obté el tipus de l'objecte (amb valors literals gràcies a `as const`).
2. `keyof typeof frontendToBackendEnumMap` obté la unió de claus: `"singleModule" | "multiModule" | "sharedModule"`.
3. L'accés indexat `T[keyof T]` obté una unió de **tots els valors** de l'objecte.

El resultat és: `"SINGLE_MODULE" | "MULTI_MODULE" | "SHARED_MODULE"`

> **Nota:** Aquest patró és extremadament útil per crear enums bidireccionals (frontend → backend) sense duplicar tipus.

---

## Repte 9: Terminologia — Union, Discriminated Union i Enum
**Concepte après:** Diferències entre union type, discriminated union i enum.

```typescript
// A: Discriminated Union (Unió discriminada)
type A =
  | { type: "a"; a: string }
  | { type: "b"; b: string }
  | { type: "c"; c: string };

// B: Union (Unió simple)
type B = "a" | "b" | "c";

// C: Enum
enum C {
  A = "a",
  B = "b",
  C = "c",
}
```

| Concepte | Tipus | Exemple | Característica clau |
|---|---|---|---|
| **Union** | `type` | `type B = "a" \| "b" \| "c"` | Unió de valors/ tipus simples |
| **Discriminated Union** | `type` | `type A = { type: "a"; a: string } \| ...` | Cada membre té un discriminant comú (`type`) que permet diferenciar-los |
| **Enum** | `enum` | `enum C { A = "a", ... }` | Conjunt de valors amb nom, existeix en temps d'execució |

> **Nota clau:** Les unions discriminades permeten el **type narrowing** automàtic: si comprovem `if (value.type === "a")`, TypeScript sap dins del bloc que el tipus és `{ type: "a"; a: string }`.

---

## Repte 10: Extreure d'una discriminated union amb Extract
**Concepte après:** Utility Type `Extract<T, U>`.

```typescript
type Event =
  | { type: "click"; event: MouseEvent }
  | { type: "focus"; event: FocusEvent }
  | { type: "keydown"; event: KeyboardEvent };

type ClickEvent = Extract<Event, { type: "click" }>;
```

`Extract<T, U>` **extreu** els membres de `T` que són assignables a `U`. Aquí, extraiem de la unió `Event` tots els membres que tenen `type: "click"`. Com que només un membre compleix la condició, el resultat és `{ type: "click"; event: MouseEvent }`.

> **Nota:** `Extract` és especialment útil amb unions discriminades per filtrar per un valor concret del discriminant.

---

## Repte 11: Excloure d'una discriminated union amb Exclude
**Concepte après:** Utility Type `Exclude<T, U>`.

```typescript
type Event =
  | { type: "click"; event: MouseEvent }
  | { type: "focus"; event: FocusEvent }
  | { type: "keydown"; event: KeyboardEvent };

type NonKeyDownEvents = Exclude<Event, { type: "keydown" }>;
```

`Exclude<T, U>` **exclou** de `T` tots els membres que són assignables a `U`. Aquí, excloem els events de tipus `"keydown"`, deixant només `{ type: "click"; event: MouseEvent } | { type: "focus"; event: FocusEvent }`.

> **Nota:** `Extract` i `Exclude` són oposats complementaris: `Extract` selecciona, `Exclude` descarta. Tots dos funcionen sobre unions.

---

## Repte 12: Obtenir el tipus del discriminador d'una discriminated union
**Concepte après:** Accés indexat sobre una unió per obtenir el discriminant.

```typescript
type Event =
  | { type: "click"; event: MouseEvent }
  | { type: "focus"; event: FocusEvent }
  | { type: "keydown"; event: KeyboardEvent };

type EventType = Event["type"];
```

Quan s'aplica un accés indexat sobre una unió, TypeScript retorna la **unió dels tipus de la propietat** per a cada membre. `Event["type"]` obté `"click" | "focus" | "keydown"`, que són tots els valors possibles del discriminant `type`.

> **Nota:** Aquest patró és útil per crear mapes o configuracions on necessitem llistar tots els valors possibles d'un discriminant sense duplicar-los manualment.

---

## Repte 13: Obtenir el tipus a partir d'un array de valors ( reforç )
**Concepte après:** Reforç de l'accés indexat amb arrays i `as const`.

```typescript
const fruits = ["apple", "banana", "orange"] as const;

type AppleOrBanana = typeof fruits[0 | 1];
type Fruit = typeof fruits[number];
```

Aquest repte és un reforç del Repte 7 amb la mateixa estructura però amb un enunciat més detallat. Les solucions són idèntiques:

- `typeof fruits[0 | 1]` → extreu els tipus de les posicions 0 i 1: `"apple" | "banana"`
- `typeof fruits[number]` → extreu el tipus de totes les posicions: `"apple" | "banana" | "orange"`

> **Nota:** Aquest patró es repeteix perquè és una de les tècniques més útils en TypeScript quotidià: transformar un array de valors coneguts en un tipus unió.

---

## Resum de Utility Types i Operators Utilitzats

| Utility / Operador | Descripció | Exemple |
|---|---|---|
| `ReturnType<T>` | Extreu el tipus de retorn d'una funció | `ReturnType<typeof myFunc>` → `string` |
| `Parameters<T>` | Extreu els paràmetres com a tupla | `Parameters<typeof makeQuery>` → `[url: string, opts?: {...}]` |
| `Awaited<T>` | Desembolica el tipus intern d'una Promise | `Awaited<Promise<string>>` → `string` |
| `keyof T` | Retorna una unió de les claus d'un tipus | `keyof typeof obj` → `"a" \| "b" \| "c"` |
| `T[K]` | Accés indexat a un tipus | `typeof obj["prop"]` → `string` |
| `T[number]` | Accés indexat a tots els elements d'un array | `typeof arr[number]` → unió d'elements |
| `Extract<T, U>` | Extreu membres de T assignables a U | `Extract<Event, { type: "click" }>` |
| `Exclude<T, U>` | Exclou membres de T assignables a U | `Exclude<Event, { type: "keydown" }>` |
| `typeof` | Obté el tipus d'una variable en temps de compilació | `typeof myFunc` → tipus de funció |
| `as const` | Marca valors com a literals (readonly + tipus literals) | `["a", "b"] as const` → tupla readonly |