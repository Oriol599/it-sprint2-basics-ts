# Explicació dels 18 Reptes de TypeScript

## Repte 1: Problema de nombres
**Concepte après:** Anotació de tipus en paràmetres de funció.

```typescript
const addTwoNumbers = (a: number, b: number) => {
  return a + b;
};
```

S'afegeix `: number` als paràmetres `a` i `b` per indicar que només accepten valors numèrics. TypeScript inferirà automàticament que el retorn també és `number`.

---

## Repte 2: Problema de paràmetre objecte
**Concepte après:** Tipatge inline d'objectes com a paràmetre.

```typescript
const addTwoNumbers = (params: { first: number; second: number }) => {
  return params.first + params.second;
};
```

En lloc de dos paràmetres separats, s'usa un sol paràmetre objecte amb les propietats `first` i `second` tipades com a `number`.

---

## Repte 3: Problema de propietats opcionals
**Concepte après:** Propietats opcionals amb `?`.

```typescript
const getName = (params: { first: string; last?: string }) => {
```

El `?` després del nom de la propietat (`last?`) la fa opcional. Pot estar present o no, i si no hi és, el seu valor serà `undefined`.

---

## Repte 4: Problema de paràmetres opcionals
**Concepte après:** Paràmetres de funció opcionals amb `?`.

```typescript
const getName = (first: string, last?: string) => {
```

Igual que amb les propietats d'objecte, el `?` fa que un paràmetre sigui opcional. Els paràmetres opcionals han d'anar **sempre al final** de la llista de paràmetres.

---

## Repte 5: Problema d'assignació de tipus a variables
**Concepte après:** Anotació de tipus en variables.

```typescript
const defaultUser: User = {
  id: 1,
  firstName: 'Jhon',
  lastName: 'Rush',
  isAdmin: true
};
```

S'afegeix `: User` després del nom de la variable per forçar que l'objecte compleixi la interfície `User`. Si falten propietats o són del tipus incorrecte, TypeScript donarà error.

---

## Repte 6: Problema d'unions
**Concepte après:** Tipus unió (`|`) per restringir valors possibles.

```typescript
role: "admin" | "user" | "super-admin";
```

L'operador `|` crea un **tipus unió**. La propietat `role` només pot tenir un dels tres valors literals: `"admin"`, `"user"` o `"super-admin"`. Qualsevol altre valor (com `"I_SHOULD_NOT_BE_ALLOWED"`) provocarà un error de TypeScript.

---

## Repte 7: Problema d'arrays
**Concepte après:** Tipatge d'arrays amb `Tipus[]`.

```typescript
posts: Post[];
```

`Post[]` indica que `posts` és un array on cada element ha de ser de tipus `Post`. És equivalent a `Array<Post>`.

---

## Repte 8: Problema d'anotacions de tipus de retorn de funció
**Concepte après:** Tipus de retorn explícit en funcions.

```typescript
const makeUser = (): User => {
```

El `: User` després dels parèntesis de paràmetres i abans de `=>` indica que la funció **ha de retornar** un objecte que compleixi la interfície `User`. Si el retorn no compleix el tipus, TypeScript donarà error.

---

## Repte 9: Problema de promeses
**Concepte après:** Tipatge de funcions asíncrones amb `Promise<T>`.

```typescript
const fetchLukeSkywalker = async (): Promise<LukeSkywalker> => {
```

Una funció `async` sempre retorna una `Promise`. El tipus de retorn `Promise<LukeSkywalker>` indica que la promesa es resoldrà amb un valor de tipus `LukeSkywalker`.

---

## Repte 10: Problema de Set
**Concepte après:** Tipus genèrics amb `Set<T>`.

```typescript
const guitarists = new Set<string>();
```

`Set<string>` és un **tipus genèric** que indica que el conjunt només pot contenir strings. Si s'intenta afegir un valor d'un altre tipus (com `2`), TypeScript donarà error.

---

## Repte 11: Problema de Record
**Concepte après:** Tipus `Record<K, V>` per a diccionaris.

```typescript
const cache: Record<string, string> = {};
```

`Record<string, string>` defineix un objecte on totes les claus són strings i tots els valors són strings. És útil per a diccionaris o memòries cau on no es coneixen les claus per endavant.

---

## Repte 12: Problema de filtratge amb typeof
**Concepte après:** Type narrowing amb `typeof`.

```typescript
const coerceAmount = (amount: number | { amount: number }) => {
  if (typeof amount === "object") {
    return amount.amount;
  }
  return amount;
};
```

`typeof` en temps d'execució permet **estretir el tipus** (type narrowing). Dins del bloc `if`, TypeScript sap que `amount` és un objecte i permet accedir a `amount.amount`. Fora del `if`, sap que és un `number`.

---

## Repte 13: Problema de blocs catch
**Concepte après:** Type narrowing amb `instanceof` en blocs catch.

```typescript
catch (e) {
  if (e instanceof Error) {
    return e.message;
  }
}
```

En un bloc `catch`, el paràmetre `e` és de tipus `unknown`. Usant `instanceof Error`, TypeScript pot estretir el tipus i permetre accedir a propietats com `message`.

---

## Repte 14: Problema d'herència amb extends
**Concepte après:** Herència d'interfícies amb `extends`.

```typescript
interface Who {
  id: string;
}
interface User extends Who {
  firstName: string;
  lastName: string;
}
interface Post extends Who {
  title: string;
  body: string;
}
interface Comment extends Who {
  comment: string;
}
```

`extends` permet que una interfície **heredi** propietats d'una altra. Aquí, `User`, `Post` i `Comment` hereten totes la propietat `id` de `Who`, evitant repetir-la (principi DRY).

---

## Repte 15: Problema d'intersecció de tipus
**Concepte après:** Intersecció de tipus amb `&`.

```typescript
const getDefaultUserAndPosts = (): User & { posts: Post[] } => {
```

L'operador `&` **combina** dos tipus en un de sol. `User & { posts: Post[] }` significa: "un objecte que té totes les propietats de `User` **i a més** una propietat `posts` que és un array de `Post`". No modifica la interfície original, sinó que crea un nou tipus combinat.

---

## Repte 16: Problema d'Omit i Pick
**Concepte après:** Utility Types `Pick<T, K>` i `Omit<T, K>`.

```typescript
type MyType = Pick<User, 'firstName' | 'lastName'>;
// Alternativa: type MyType = Omit<User, 'id'>;
```

- `Pick<User, 'firstName' | 'lastName'>` crea un tipus **només** amb les propietats indicades.
- `Omit<User, 'id'>` crea un tipus amb **totes les propietats excepte** les indicades.

Són **Utility Types** incorporats a TypeScript per manipular tipus existents.

---

## Repte 17: Problema de tipus de funció
**Concepte après:** Tipatge de funcions com a paràmetres (callbacks).

```typescript
const addListener = (onFocusChange: (isFocused: boolean) => void) => {
```

`(isFocused: boolean) => void` és la **signatura d'una funció**: rep un paràmetre `isFocused` de tipus `boolean` i no retorna res (`void`). Això permet tipar correctament els callbacks.

---

## Repte 18: Problema de tipus de funció amb promeses
**Concepte après:** Tipatge de funcions asíncrones com a paràmetres.

```typescript
const createThenGetUser = async (
  createUser: () => Promise<string>,
  getUser: (id: string) => Promise<User>,
): Promise<User> => {
```

- `createUser: () => Promise<string>`: funció que no rep paràmetres i retorna una promesa que es resol amb un `string`.
- `getUser: (id: string) => Promise<User>`: funció que rep un `id` de tipus `string` i retorna una promesa que es resol amb un `User`.

Combina el tipatge de funcions (repte 17) amb el tipatge de promeses (repte 9).