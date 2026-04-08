<!-- @/DrawerRoot.vue -->
<template>
  <DialogRoot v-model:open="open" :modal="modal">
    <slot :open="open" />
  </DialogRoot>
</template>

<script setup lang="ts">
import { DialogRoot } from 'reka-ui';
import { watch, computed, toRefs } from 'vue';
import { provideDrawerRootContext } from './context';
import { type DrawerRootEmits, type DrawerRootProps, useDrawer } from './controls';
import { CLOSE_THRESHOLD, SCROLL_LOCK_TIMEOUT, TRANSITIONS } from './constants';
import './style.css';

const props = withDefaults(defineProps<DrawerRootProps>(), {
  open: undefined,
  defaultOpen: undefined,
  fixed: undefined,
  dismissible: true,
  activeSnapPoint: undefined,
  snapPoints: undefined,
  shouldScaleBackground: undefined,
  setBackgroundColorOnScale: true,
  closeThreshold: CLOSE_THRESHOLD,
  fadeFromIndex: undefined,
  nested: false,
  modal: true,
  scrollLockTimeout: SCROLL_LOCK_TIMEOUT,
  direction: 'bottom',
  handleOnly: false,
});

const emit = defineEmits<DrawerRootEmits>();

const slots = defineSlots<{
  default: (props: { open: typeof isOpen.value }) => any;
}>();

const fadeFromIndex = computed(
  () => props.fadeFromIndex ?? (props.snapPoints && props.snapPoints.length - 1),
);

const open = defineModel<boolean>('open', {
  default: false,
});

const emitHandlers = {
  emitDrag: (percentageDragged: number) => emit('drag', percentageDragged),
  emitRelease: (open: boolean) => emit('release', open),
  emitClose: () => emit('close'),
  emitOpenChange: (o: boolean) => {
    emit('update:open', o);

    setTimeout(() => {
      emit('animationEnd', o);
    }, TRANSITIONS.DURATION * 1000);
  },
  emitWillClose: (willClose: boolean) => emit('willClose', willClose),
};

const { closeDrawer, hasBeenOpened, modal, isOpen } = provideDrawerRootContext(
  useDrawer({
    ...emitHandlers,
    ...toRefs(props),
    fadeFromIndex,
    open,
  }),
);

watch(
  open,
  (o) => {
    if (o) {
      hasBeenOpened.value = true;
    } else {
      closeDrawer();
    }
  },
  { immediate: true },
);

defineExpose({
  open: isOpen,
});
</script>
