**@fuyeor/vue-vaul** —— An enhanced, enterprise-ready fork of `vaul-vue`. This is an unstyled drawer component for Vue that can be used as a Dialog replacement on tablet and mobile devices.

This fork maintains the core functionality while introducing critical enhancements for complex interactive scenarios, ensuring it meets military-grade project standards for stability and developer experience.

## Why this Fork?

The original `vaul-vue` provides a solid foundation but lacks crucial events for building predictive and highly responsive user interfaces. This fork addresses these gaps.

### Key Enhancements ✨

- **`will-close` Event**: A new event that fires in real-time during a drag gesture. It predicts whether the drawer will close if the user releases it at that moment, based on both drag distance and velocity. This allows for creating visual feedback (e.g., changing a handle's color) to significantly improve UX.

## Installation

```bash
pnpm add @fuyeor/vue-vaul
```

```bash
npm install @fuyeor/vue-vaul
```

```bash
yarn add @fuyeor/vue-vaul
```

## Basic Usage

```vue
<script setup lang="ts">
import { DrawerContent, DrawerOverlay, DrawerPortal, DrawerRoot, DrawerTrigger } from '@fuyeor/vue-vaul'
</script>

<template>
  <DrawerRoot>
    <DrawerTrigger> Open </DrawerTrigger>
    <DrawerPortal>
      <DrawerOverlay class="sheet-backdrop modal-backdrop" />
      <DrawerContent class="sheet-content">
        <p>Content</p>
      </DrawerContent>
    </DrawerPortal>
  </DrawerRoot>
</template>
```

## Advanced Usage: Predictive UI with `will-close`

Here’s how to use the new `@will-close` event to change the drag handle's color, indicating to the user that releasing will close the drawer.

```vue
<script setup lang="ts">
import { ref, watch } from 'vue'
import { DrawerContent, DrawerHandle, DrawerOverlay, DrawerPortal, DrawerRoot, DrawerTrigger } from '@fuyeor/vue-vaul'

// State to control the drawer's visibility
const isOpen = ref(false)
// State to track if the drawer is predicted to close
const isWillClose = ref(false)

// Update our state when the event is emitted
function onWillClose(close: boolean) {
  isWillClose.value = close
}

// Ensure the indicator is reset when the drawer is closed
watch(isOpen, (newOpenState) => {
  if (!newOpenState) {
    isWillClose.value = false
  }
})
</script>

<template>
  <DrawerRoot v-model:open="isOpen" should-scale-background @will-close="onWillClose">
    <DrawerTrigger> Open Drawer </DrawerTrigger>
    <DrawerPortal>
      <DrawerOverlay class="fixed inset-0 bg-black/40" />
      <DrawerContent class="bg-gray-100 flex flex-col rounded-t-lg h-[96%] mt-24 fixed bottom-0 left-0 right-0">
        <div class="p-4 bg-white rounded-t-lg flex-1">
          <DrawerHandle
            class="mb-8 mt-2"
            :style="{ backgroundColor: isWillClose ? '#EF4444' /* red-500 */ : '#D4D4D8' /* zinc-300 */ }"
          />
          <p>Drag down to close. The handle will turn red when it's about to close.</p>
        </div>
      </DrawerContent>
    </DrawerPortal>
  </DrawerRoot>
</template>
```

## API

### Events

| Event        | Payload               | Description                                                                                                                              |
| :----------- | :-------------------- | :--------------------------------------------------------------------------------------------------------------------------------------- |
| `will-close` | `(willClose: boolean)` | Emits during a drag gesture. `true` if releasing will close the drawer (based on distance/velocity), otherwise `false`. |

## Credits

This work is built upon incredible open-source projects. All credits go to:

-   [Emil Kowalski](https://emilkowal.ski/) for the original [Vaul library](https://github.com/emilkowalski/vaul) for React.
-   The creators of the initial `vaul-vue` port.
-   [Reka UI](https://www.reka-ui.com/) for the Dialog primitive used under the hood.