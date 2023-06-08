## [9. type-modifiers 02-artifact-assertions](https://github.com/LearningTypeScript/projects/tree/main//projects/type-modifiers/modifiers-of-the-types/02-artifact-assertions)

### 문제

See the artifacts variable already written for you. You must give us both an ArtifactName type describing the names of those object, and a getArtifactType function that takes in a name and returns one of the four possible types.

### Specification

Export an ArtifactName type equivalent to a union of the literal keys of artifacts.

Export an getArtifactType function with a single parameter of that ArtifactName type. It should return the type property of the corresponding artifact. Its return type should be a union of the four possible string literals.

### 풀이 및 느낀 점

artifacts 를 어떻게 타입으로 만들지 생각을 못했다.\
`as const`로 type의 value가 string이 아닌 4가지만 되도록 하고,\
ArtifactName를 하드코딩할 필요 없이 `keyof typeof` 이렇게 쓰면 되는 거였네...\
리턴값이 계속 4가지 값이 아닌 string로 돼서 왠만하면 쓰지 말라는 `as 어서션`도 썼다 \
어렵다...😂

```ts
// Write your types and function below! ✨
// You'll need to export them so the tests can run it.

const artifacts = {
  "Black Garnet": {
    type: "magic",
  },
  "Castle TypeScript": {
    type: "fortress",
  },
  "Cosmic Key": {
    type: "device",
  },
  "Power Sword": {
    type: "sword",
  },
  Starseed: {
    type: "magic",
  },
  "Sword of the Ancients": {
    type: "sword",
  },
}; // as const

export type ArtifactName = {
  "Black Garnet": { type: "magic" | "fortress" | "device" | "sword" };
  "Castle TypeScript": { type: "magic" | "fortress" | "device" | "sword" };
  "Cosmic Key": { type: "magic" | "fortress" | "device" | "sword" };
  "Power Sword": { type: "magic" | "fortress" | "device" | "sword" };
  Starseed: { type: "magic" | "fortress" | "device" | "sword" };
  "Sword of the Ancients": { type: "magic" | "fortress" | "device" | "sword" };
};

export function getArtifactType(key: keyof ArtifactName) {
  return artifacts[key].type as "magic" | "fortress" | "device" | "sword";
}
```

### 레퍼런스

```ts
const artifacts = {
  "Black Garnet": {
    type: "magic",
  },
  "Castle TypeScript": {
    type: "fortress",
  },
  "Cosmic Key": {
    type: "device",
  },
  "Power Sword": {
    type: "sword",
  },
  Starseed: {
    type: "magic",
  },
  "Sword of the Ancients": {
    type: "sword",
  },
} as const;

export type ArtifactName = keyof typeof artifacts;

export function getArtifactType(name: ArtifactName) {
  return artifacts[name].type;
}
```
