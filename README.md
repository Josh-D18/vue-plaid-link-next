# Vue Plaid Link Next

[![npm version](https://badge.fury.io/js/@sense%2Fvue-plaid-link.svg)](https://www.npmjs.com/package/@sense/vue-plaid-link)

A lightweight, open-source Vue integration for [Plaid Link](https://plaid.com/docs/link/), built around Plaid's browser SDK.

**Vue Plaid Link Next** provides a Vue-native composable API, TypeScript support, and Plaid Link lifecycle management while keeping the underlying Plaid SDK isolated from the rest of your application.

> **Origin:** This project was adapted from [vue-plaid-link](https://github.com/jclaessens97/vue-plaid-link) by Jeroen Claessens and incorporates code from the original project under the MIT License. See [LICENSE](LICENSE) for attribution and licensing information.

## Why This Project Exists

Integrating Plaid Link into Vue applications should be simple.

Vue Plaid Link Next provides a thin abstraction around Plaid's browser SDK so Vue applications can interact with Plaid Link through composables rather than directly managing the global `window.Plaid` API.

The project focuses on:

* Vue-native composables
* TypeScript support
* Plaid Link lifecycle management
* Strongly typed configuration and callback metadata
* A lightweight abstraction around Plaid's browser SDK
* Reusable integration across Vue applications

## Compatibility

* Vue 3.2+
* Plaid Link Web SDK

## Installation

### npm

```bash
npm install @sense/vue-plaid-link
```

### yarn

```bash
yarn add @sense/vue-plaid-link
```

### pnpm

```bash
pnpm add @sense/vue-plaid-link
```

## Documentation

For complete information about Plaid Link configuration, events, OAuth, and `link_token` creation, refer to the official Plaid documentation:

* [Plaid Link Documentation](https://plaid.com/docs/link/)
* [Plaid Link Web Documentation](https://plaid.com/docs/link/web/)
* [Link Token Create API](https://plaid.com/docs/api/tokens/#linktokencreate)

## Using Vue Composables

The recommended approach is to use the `usePlaidLink` composable.

The composable provides a Vue-friendly interface for interacting with Plaid Link while handling the underlying SDK lifecycle.

> **Note:** `token` can initially be `null` and updated once your application receives a `link_token` from your backend.

### Basic Example

```vue
<template>
  <button :disabled="!ready" @click="open">
    Connect a bank account
  </button>
</template>

<script lang="ts" setup>
import { usePlaidLink } from '@sense/vue-plaid-link';

const { open, ready } = usePlaidLink({
  token: '<GENERATED_LINK_TOKEN>',
  onSuccess: (public_token, metadata) => {
    // Send the public token to your backend.
  },
});
</script>
```

## Architecture

The package intentionally keeps the Plaid SDK behind a small Vue-specific abstraction.

```text
Vue Application
      │
      ▼
usePlaidLink()
      │
      ▼
Vue Plaid Link Next
      │
      ▼
window.Plaid
      │
      ▼
Plaid Link Web SDK
```

This allows application code to interact with Plaid through a Vue-friendly API without directly coupling components to the global Plaid SDK.

## Examples

See the [`examples`](examples) directory for complete examples:

* [`examples/simple.vue`](examples/simple.vue) — Minimal composable integration
* [`examples/composable.vue`](examples/composable.vue) — Composable with callbacks
* [`examples/oauth.vue`](examples/oauth.vue) — OAuth redirect handling
* [`examples/component.vue`](examples/component.vue) — Pre-built component integration

## API

### `usePlaidLink`

#### Arguments

| Key                   | Type                                                                                      |
| --------------------- | ----------------------------------------------------------------------------------------- |
| `token`               | `string \| null`                                                                          |
| `receivedRedirectUri` | `string \| undefined`                                                                     |
| `onSuccess`           | `(public_token: string, metadata: PlaidLinkOnSuccessMetadata) => void`                    |
| `onExit`              | `(error: PlaidLinkError \| null, metadata: PlaidLinkOnExitMetadata) => void`              |
| `onEvent`             | `(eventName: PlaidLinkStableEvent \| string, metadata: PlaidLinkOnEventMetadata) => void` |
| `onLoad`              | `() => void`                                                                              |

See the exported types in [`src/types`](src/types).

#### Return Value

| Key     | Type                                                            |
| ------- | --------------------------------------------------------------- |
| `open`  | `() => void`                                                    |
| `ready` | `boolean`                                                       |
| `error` | `ErrorEvent \| null`                                            |
| `exit`  | `(options?: { force: boolean }, callback?: () => void) => void` |

## Using the `PlaidLink` Component

If you cannot use Vue composables, the package also provides a `PlaidLink` component.

```vue
<template>
  <PlaidLink
    :token="token"
    :on-success="onSuccess"
    :on-event="onEvent"
  >
    Connect a bank account
  </PlaidLink>
</template>

<script lang="ts" setup>
import PlaidLink from '@sense/vue-plaid-link';
import type {
  PlaidLinkOnEvent,
  PlaidLinkOnSuccess,
} from '@sense/vue-plaid-link';

// ...
</script>
```

## Contributing

Contributions are welcome.

If you find a bug, have an improvement, or want to add support for additional Plaid Link functionality:

1. Fork the repository.
2. Create a feature branch.
3. Make your changes.
4. Add or update tests where appropriate.
5. Open a pull request.

Please keep contributions focused on providing a lightweight, maintainable Vue integration around Plaid Link.

## License

This project is licensed under the MIT License.

Vue Plaid Link Next is based in part on [`vue-plaid-link`](https://github.com/jclaessens97/vue-plaid-link) by Jeroen Claessens.

The original copyright notice and MIT License terms are preserved in [LICENSE](LICENSE).
