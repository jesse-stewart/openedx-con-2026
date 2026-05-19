<script setup>
import { computed } from 'vue'
import { useSlideContext } from '@slidev/client'

const { $page, $slidev } = useSlideContext()

const shouldShowLogo = computed(() => {
  const currentPage = $page.value
  const layout = $slidev.nav.currentLayout

  // Hide on slides 1 and 2, and on api-section layout
  if (currentPage <= 2) return false
  if (layout === 'api-section') return false

  return true
})
</script>

<template>
  <div v-if="shouldShowLogo" class="openedx-logo">
    <img src="/openedx-logo.svg" alt="Open edX Logo" />
  </div>
</template>

<style scoped>
.openedx-logo {
  position: absolute;
  top: 0.5rem;
  right: 0.5rem;
  z-index: 50;
  pointer-events: none;
}

.openedx-logo img {
  height: 1.25rem;
  width: auto;
  opacity: 0.75;
}
</style>
