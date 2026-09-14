# tool-gen-storage

Tiny typed storage adapters with optional in-memory caching.

## Install

```sh
npm install @arylo-scripts/tool-gen-storage
```

## Usage

```ts
import { genStorage, genStorageAsync } from '@arylo-scripts/tool-gen-storage'

const storage = genStorage<string>({
  save: (key, value) => localStorage.setItem(key, value),
  load: (key) => localStorage.getItem(key),
})

storage.set('theme', 'dark')
const theme = storage.get('theme', 'light')

const asyncStorage = genStorageAsync<string>({
  save: async (key, value) => chrome.storage.local.set({ [key]: value }),
  load: async (key) => (await chrome.storage.local.get(key))[key],
})

await asyncStorage.set('theme', 'dark')
const savedTheme = await asyncStorage.get('theme', 'light')
```

Values are serialized as JSON by default, and each storage instance keeps an
in-memory cache unless `useCache` is set to `false`.

## API

- `genStorage(options?)`
- `genStorageAsync(options?)`

Both factories return a storage object with `get(key, defaultValue?)` and
`set(key, value)` methods.

## Development

```sh
npm install
npm run lint
npm run build
```
