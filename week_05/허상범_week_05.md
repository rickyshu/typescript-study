## Chapter 9. Type Modifiers

### Step 1: Fabulous Secret Powers

> https://github.com/LearningTypeScript/projects/tree/main//projects/type-modifiers/modifiers-of-the-types/01-fabulous-secret-powers

```ts
// type으로 배열을 만들면, 변수를 선언해 as const 한 것보다 컴파일 시 용량에 유리하다.
// (기존에 사용하는 배열이 없는 경우에..)
type Names = ['Beast Man', 'Evil-Lyn', 'He-Man', 'Skeletype', 'Teela Na']
type Powers = [
  'speed',
  'magic',
  'strength',
  'agility',
  'durability',
  'mind control',
  'animal control',
  'telepathy',
  'telekinesis',
  'teleportation',
  'transformation'
]
type Sides = ['good', 'evil']

export interface Character {
  name: Names[number]
  powers: Powers[number][]
  side: Sides[number]
}

export function announceCharacter(raw: string): Character {
  const { name, powers, side } = JSON.parse(raw) as Character
  console.log(`I am ${name}.`)
  console.log(`My powers are: ${powers.join(', ')}.`)
  console.log(`I am ${side}.`)

  return { name, powers, side }
}
```

<br>

## Step 2: Artifact Assertions

> https://github.com/LearningTypeScript/projects/tree/main//projects/type-modifiers/modifiers-of-the-types/02-artifact-assertions

```ts
// as const로 readonly로 변환
// keyof typeof => 객체의 키를 가져오는 방법
const artifacts = {
  'Black Garnet': {
    type: 'magic',
  },
  'Castle TypeScript': {
    type: 'fortress',
  },
  'Cosmic Key': {
    type: 'device',
  },
  'Power Sword': {
    type: 'sword',
  },
  Starseed: {
    type: 'magic',
  },
  'Sword of the Ancients': {
    type: 'sword',
  },
} as const

export type ArtifactName = keyof typeof artifacts

export function getArtifactType(artifactName: ArtifactName) {
  return artifacts[artifactName].type
}
```

<br>

### Type Force

> https://github.com/LearningTypeScript/projects/tree/main/projects/type-modifiers/type-force

```ts
// as const 로 튜플로 리턴하기
// keyof typeof로 객체의 키를 타입으로 사용하기
interface Character {
  name: string
  power: number
  flying: boolean
  toughness: number
}

const mutationsLibrary = {
  energy: (character: Character) => {
    hero.power *= 1.25
    hero.flying = true
  },
  healing: (character: Character) => {
    hero.toughness *= 2
  },
  luck: (character: Character) => {
    hero.power *= 1.25
    hero.toughness *= 1.25
  },
  flight: (character: Character) => {
    hero.flying = true
  },
  strength: (character: Character) => {
    hero.power *= 2
  },
  wings: (character: Character) => {
    hero.flying = true
    hero.toughness *= 0.9
  },
} as const

type mutation = keyof typeof mutationsLibrary

function createCharacter(name: string, mutations: mutation[]): Character {
  const character = {
    flying: false,
    name,
    power: 1,
    toughness: 1,
  }

  for (const mutation of mutations) {
    mutationsLibrary[mutation](character)
  }

  return character
}

interface Candidate {
  name: string
  mutations: mutation[]
}

export function duel(good: Candidate, bad: Candidate) {
  const hero = createCharacter(good.name, good.mutations)
  const villain = createCharacter(bad.name, bad.mutations)
  const heroScore = hero.power / villain.toughness
  const villainScore = villain.power / hero.toughness

  return heroScore > villainScore ? (['hero', hero] as const) : (['villain', villain] as const)
}
```

<br>

## Chapter 10. Generics

### Step 1: Unique

> https://github.com/LearningTypeScript/projects/tree/main//projects/generics/hidash/01-unique

```ts
export function unique<T extends K, K>(...items: T[]): K[] {
  const set = new Set<K>()

  function dfs(...items: T[]): void {
    console.log(items)
    for (const item of items) {
      Array.isArray(item) ? dfs(...item) : set.add(item)
    }
  }

  dfs(...items)

  return Array.from(set)
}
```

<br>

### Step 2: Zip

> https://github.com/LearningTypeScript/projects/tree/main//projects/generics/hidash/01-unique

```ts
export function zip<T, K>(a: T[], b: K[]): (T | K)[] {
  const answer: (T | K)[] = []

  for (let i = 0; i < Math.max(a.length, b.length); i++) {
    a[i] !== undefined && answer.push(a[i])
    b[i] !== undefined && answer.push(b[i])
  }
  return answer
}
```

<br>

### Step 3: Pick

> https://github.com/LearningTypeScript/projects/tree/main//projects/generics/hidash/03-pick

```ts
export function pick<T, K extends keyof T>(obj: T, key: K): T[K] {
  return obj[key]
}
```

<br>

### Step 4: Pick Many

> https://github.com/LearningTypeScript/projects/tree/main//projects/generics/hidash/04-pickmany

```ts
export function pickMany<T, K extends keyof T>(obj: T, arrKey: K[]): T[K][] {
  return arrKey.map(key => obj[key])
}
```

<br>
