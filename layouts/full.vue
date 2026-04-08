<template>
  <div class="slidev-layout layout-full">
    <slot />
    <div class="layout-full-overlay" />
  </div>
</template>

<style scoped>
.layout-full {
  background: transparent;
  width: 100%;
  height: 100%;
  padding: 0;
  margin: 0;
  overflow: hidden;
  position: relative;
}

.layout-full-overlay {
  position: absolute;
  inset: 0;
  pointer-events: none;
  background-image: url('/conference-waves.png');
  background-repeat: repeat-x;
  background-position: left bottom;
  background-size: 28px 41px;
}
</style>
