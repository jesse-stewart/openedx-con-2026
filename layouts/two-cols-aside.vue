<template>
  <div class="slidev-layout two-cols-aside">
    <div class="aside">
      <slot name="aside" />
    </div>
    <div class="main">
      <slot />
    </div>
  </div>
</template>

<style scoped>
.two-cols-aside {
  display: grid;
  grid-template-columns: minmax(0, 1fr) minmax(0, 2fr);
  gap: 2rem;
  align-items: start;
}

.aside,
.main {
  min-width: 0;
  overflow: hidden;
}
</style>
