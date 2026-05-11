<template>
  <div class="title-slide">

    <!-- ── Panels (background layers only) ──────────────────── -->
    <div class="left-panel">
      <img src="/conference-banner.png" class="city-banner" style="view-transition-name: city" alt="" />
      <div class="openedx-wrap">
        <img src="/openedx-logo-2026.png" alt="Open edX® 2026" />
      </div>
      <div class="logos">
        <img src="/wgu-logo.png"      alt="WGU" />
        <img src="/openedx-logo.svg"  alt="Open edX®" />
      </div>
    </div>
    <div class="right-panel" />

    <!-- ── Slide-level overlays ──────────────────────────────── -->

    <!-- Title "BEYOND TEST COVERAGE" -->
    <div class="title-text" style="view-transition-name: title">
      <div>Beyond Test Coverage</div>
    </div>

    <!-- Subtitle dark banner (spans both panels) -->
    <div class="subtitle-banner" style="view-transition-name: subtitle">
      <div>
        <span>E2E</span><span class="dot"> • </span><span>A11y</span><span class="dot"> • </span><span>Documentation</span><span class="dot"> • </span><br>
        <span>Visual Regression Testing</span>
      </div>
    </div>

    <!-- Salt Lake City label -->
    <div class="slc-label" style="view-transition-name: slc-label">Salt Lake City, Utah</div>

    <!-- Dashed arc (exact Illustrator coordinates, 1920×1080 viewBox) -->
    <svg class="path-arc" style="view-transition-name: arc" viewBox="0 0 1920 1080" preserveAspectRatio="none">
      <path class="arc-solid" d="M226,650.1c-0.1,2-0.2,4-0.2,6"/>
      <path class="arc-dash"  d="M2899.6,626.5c-56.9,-181.8,-632.7,-324.7,-1334.6,-324.7c-716.9,0,-1302.2,149,-1337.5,336.4"/>
    </svg>

    <!-- Speaker info (right panel) -->
    <img
      src="/headshot.png"
      class="headshot"
      style="view-transition-name: headshot"
      alt="Jesse Stewart"
    />
    <div class="speaker-name" style="view-transition-name: speaker-name">JESSE STEWART</div>
    <div class="speaker-meta">
      <div class="speaker-title" style="view-transition-name: speaker-title">STAFF SOFTWARE ENGINEER</div>
      <div class="speaker-url"  style="view-transition-name: speaker-url">www.JesseStewart.com</div>
    </div>
  </div>
</template>

<style scoped>
/* ── Slide container ── */
.title-slide {
  width: 100%;
  height: 100%;
  position: relative;
  display: flex;
  overflow: hidden;
  font-family: var(--font-sans);
}

/* ── Panels ── */
.left-panel {
  width: 66%;
  flex-shrink: 0;
  position: relative;
  background: var(--color-cyan-light);
}

.right-panel {
  flex: 1;
  background: white;
}

/* City banner: 200% wide, left edge anchored so city buildings show */
.city-banner {
  position: absolute;
  bottom: 0;
  left: 0;
  width: 200%;
  max-width: none;
  z-index: 1;
}

.openedx-wrap {
  position: absolute;
  z-index: 2;
  bottom: 34%;
  left: 70%;
  transform: translateX(-50%);
}
.openedx-wrap img { width: 360px; height: auto; max-width: none; }

/* ── Title ── */
.title-text {
  position: absolute;
  left: 2.5%;
  top: 13%;
  width: 300px;
  text-align: right;
  font-family: var(--font-display-uber);
  font-size: 32px;
  font-weight: 900;
  line-height: 1.2;
  text-transform: uppercase;
  color: var(--color-blue-dark);
  z-index: 10;
}

/* ── Subtitle dark banner ── */
.subtitle-banner {
  position: absolute;
  top: 7.7%;       /* 83 / 1080 */
  left: 35.5%;     /* 681.2 / 1920 */
  width: 60%;      /* (1832.7 - 681.2) / 1920 */
  height: 25%;     /* (352.7 - 83) / 1080 */
  background: var(--color-blue-dark);
  clip-path: polygon(4.56% 0%, 100% 0%, 95.45% 100%, 0% 100%);
  display: flex;
  align-items: center;
  padding-left: 4%;
  z-index: 9;
  font-family: var(--font-display);
  font-style: italic;
  font-size: 32px;
  color: white;
  line-height: 1.2;
}
.subtitle-banner .dot { color: var(--color-cyan-light); }

/* ── SLC label ── */
.slc-label {
  position: absolute;
  text-transform: uppercase;
  top: 55%;
  left: 0.5%;
  background: var(--color-blue-dark);
  color: white;
  font-family: var(--font-display);
  font-style: italic;
  font-weight: 900;
  font-size: 16px;
  letter-spacing: 1px;
  padding: 8px 16px;
  z-index: 11;
  clip-path: polygon(4.56% 0%, 100% 0%, 95.45% 100%, 0% 100%);
}

/* ── Path arc ── */
.path-arc {
  position: absolute;
  left: 3%;
  width: 80%;
  z-index: 8;
  pointer-events: none;
  overflow: visible;
}
.arc-solid {
  fill: none;
  stroke: var(--color-blue);
  stroke-width: 14;
  stroke-linecap: round;
}
.arc-dash {
  fill: none;
  stroke: var(--color-blue);
  stroke-width: 14;
  stroke-linecap: round;
  stroke-linejoin: round;
  stroke-dasharray: 12.0445 24.0889;
}

/* ── Speaker info ── */
.headshot {
  position: absolute;
  left: 70%;
  top: 37.4%;
  width: 76px;
  height: 76px;
  border-radius: 50%;
  object-fit: cover;
  z-index: 10;
}

.speaker-name {
  position: absolute;
  left: 70%;
  top: 53%;
  font-family: var(--font-display-uber);
  font-size: 24px;
  font-weight: 900;
  color: var(--color-blue);
  text-transform: uppercase;
  z-index: 10;
  white-space: nowrap;
}

.speaker-meta {
  position: absolute;
  left: 70%;
  top: 60%;
  z-index: 10;
}

.speaker-title {
  font-family: var(--font-sans);
  font-size: 16px;
  font-weight: 700;
  color: var(--color-blue);
  text-transform: uppercase;
  letter-spacing: 1px;
}

.speaker-url {
  font-family: var(--font-mono);
  font-size: 16px;
  color: var(--color-blue);
}

/* ── Logos ── */
.logos {
  position: absolute;
  bottom: 30%;
  left: 70%;
  transform: translateX(-50%);
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 12px;
  z-index: 10;
}
.logos img { width: 120px; }
</style>
