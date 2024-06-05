# VueJS Best Practices

VueJS applications should be approachable, component-driven, reactive without
being confusing, and fast to build locally. Keep state ownership clear and make
components easy to reuse.

## Core Principles

1. Keep the simple path simple.
2. Use progressive enhancement where possible.
3. Keep component boundaries clear.
4. Keep state close to where it is used.
5. Prefer explicit data flow.
6. Optimize developer feedback loops.
7. Avoid framework complexity the product does not need.

## Components

1. Keep components focused.
2. Use props for parent-to-child data.
3. Use emits for child-to-parent communication.
4. Avoid mutating props.
5. Move repeated behavior into composables.
6. Keep templates readable.
7. Keep component APIs small.

```vue
<script setup>
defineProps({
  title: {
    type: String,
    required: true,
  },
});
</script>

<template>
  <h1>{{ title }}</h1>
</template>
```

## Reactivity

1. Use `ref` for single reactive values.
2. Use `reactive` for grouped state.
3. Use `computed` for derived values.
4. Use watchers only for side effects.
5. Avoid duplicating state that can be derived.
6. Keep global state intentional.

```js
const firstName = ref('');
const lastName = ref('');

const fullName = computed(() => `${firstName.value} ${lastName.value}`.trim());
```

## State Management

1. Keep local state local.
2. Use a store when state must be shared across unrelated components.
3. Keep stores focused by domain.
4. Represent async state explicitly: loading, success, empty, and error.
5. Avoid storing server data in multiple places.
6. Reset state deliberately when leaving workflows.

## Routing

1. Keep route names stable.
2. Use route params for resource identity.
3. Use query params for filters and view state.
4. Protect private routes with route guards or framework middleware.
5. Keep page-level data loading predictable.

## Forms

1. Use labels for every input.
2. Validate at the right boundary.
3. Show field-specific errors.
4. Disable submit only when the user has a clear path forward.
5. Preserve input after failed validation.
6. Keep keyboard navigation working.

## Performance

1. Test production builds, not only the dev server.
2. Split routes when the app grows.
3. Avoid unnecessary watchers.
4. Avoid rendering huge lists without virtualization.
5. Keep computed values cheap.
6. Measure before optimizing.

## Tooling

1. Use Vite for fast development when the project allows it.
2. Run the project formatter.
3. Run linting before committing.
4. Run unit tests and component tests where configured.
5. Keep build configuration small until customization is necessary.

```bash
npm run build
npm run test
```

## Review Checklist

1. Is the component focused?
2. Is state ownership clear?
3. Are async states handled?
4. Are props treated as read-only?
5. Are forms accessible?
6. Was the production build tested?
7. Is added configuration justified?

---

### Made with ❤️ by Nsisong Labs
