<script setup lang="ts">
import { ref,watch } from 'vue'
import { DrawerContent, DrawerHandle, DrawerOverlay, DrawerPortal, DrawerRoot, DrawerTrigger } from '@fuyeor/vue-vaul'

const isWillClose = ref(false)
const isOpen = ref(false)

function onWillClose(close: boolean) {
  isWillClose.value = close
}

watch(isOpen, (newOpenState) => {
  if (!newOpenState) {
    isWillClose.value = false
  }
})
</script>

<template>
   <DrawerRoot v-model:open="isOpen" should-scale-background @will-close="onWillClose">
    <DrawerTrigger
      class="rounded-full bg-white px-4 py-2.5 text-sm font-semibold text-gray-900 shadow-sm ring-1 ring-inset ring-gray-300 hover:bg-gray-50"
    >
      Open Drawer
    </DrawerTrigger>
    <DrawerPortal>
      <DrawerOverlay class="fixed inset-0 bg-black/40" />
      <DrawerContent
        class="bg-gray-100 flex flex-col rounded-t-[10px] h-full mt-24 max-h-[96%] fixed bottom-0 left-0 right-0"
      >
        <div class="p-4 bg-white rounded-t-[10px] flex-1">
          <DrawerHandle
            data-testid="handle"
            class="mb-8 mt-2"
            :style="{ backgroundColor: isWillClose ? '#AEA4E4' : '#D4D4D8' }"
          />

          <div class="max-w-md mx-auto">
            <h2 id="radix-:R3emdaH1:" class="font-medium mb-4">
              Drawer for Vue.
            </h2>
            <p class="text-gray-600 mb-2">
              This component can be used as a Dialog replacement on mobile and tablet devices.
            </p>
            <p class="text-gray-600 mb-2">
              It comes unstyled, has gesture-driven animations, and is made by
              <a href="https://emilkowal.ski/" class="underline" target="_blank">Emil Kowalski</a>.
            </p>
            <p class="text-gray-600 mb-8">
              It uses
              <a
                href="https://www.radix-ui.com/docs/primitives/components/dialog"
                class="underline"
                target="_blank"
              >Radix's Dialog primitive</a>
              under the hood and is inspired by
              <a
                href="https://twitter.com/devongovett/status/1674470185783402496"
                class="underline"
                target="_blank"
              >this tweet.</a>
            </p>
          </div>
        </div>
      </DrawerContent>
    </DrawerPortal>
  </DrawerRoot>
</template>
