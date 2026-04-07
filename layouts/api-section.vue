<template>
  <div class="api-section">
    <img src="/conference-banner.png" class="city-banner" alt="" />
    <div class="content">
      <slot />
    </div>
  </div>
</template>

<style scoped>
.api-section {
  width: 100%;
  height: 100%;
  position: relative;
  overflow: hidden;
  background: var(--color-cyan-light);
  display: flex;
  align-items: center;
  justify-content: center;
  font-family: var(--font-sans);
}

.city-banner {
  position: absolute;
  bottom: 0;
  left: 0;
  width: 120%;
  max-width: none;
  pointer-events: none;
}

.content {
  position: relative;
  z-index: 2;
  text-align: center;
  padding-bottom: 10vh;
  width: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}

.content :deep(h1) {
  font-family: var(--font-display-uber);
  font-weight: 900;
  font-size: 4em;
  letter-spacing: 0.06em;
  text-transform: uppercase;
  color: var(--color-blue-dark);
  margin: 0;
}

.content :deep(p) {
  font-family: var(--font-display);
  color: var(--color-blue);
  font-size: 1.2em;
  margin-top: 0.5em;
}

.content :deep(code) {
  font-family: var(--font-mono);
  background: transparent;
  color: var(--color-blue-dark);
  padding: 0;
}
</style>
