<!-- Button.vue -->
<template>
  <Primitive :as="as" :as-child="asChild" :class="buttonClasses">
    <slot />
  </Primitive>
</template>

<script setup lang="ts">
import type { HTMLAttributes } from 'vue'
import { Primitive, type PrimitiveProps } from 'radix-vue'
import { type ButtonVariants, buttonVariants } from './buttonVariants'
import { cn } from '@/lib/utils'

interface Props extends PrimitiveProps {
  variant?: ButtonVariants['variant']
  size?: ButtonVariants['size']
  as?: string
  class?: HTMLAttributes['class']
}

const props = withDefaults(defineProps<Props>(), {
  as: 'button',
})

const buttonClasses = cn(buttonVariants({ variant: props.variant, size: props.size }), props.class)
</script>
