<template>
  <div class="about-slide">

    <!-- ── Panels (background layers only) ──────────────────── -->
    <div class="left-panel">
      <img src="/conference-banner.png" class="city-banner" style="view-transition-name: city" alt="" />
    </div>
    <div class="right-panel">
      <img src="/map.svg"              class="map"         alt="" />
    </div>

    <!-- ── Slide-level overlays ──────────────────────────────── -->

    <!-- Dashed arc (exact Illustrator coordinates, 1920×1080 viewBox) -->
    <svg class="path-arc" style="view-transition-name: arc" viewBox="0 0 1920 1080" preserveAspectRatio="none">
      <path class="arc-solid" d="M226,650.1c-0.1,2-0.2,4-0.2,6"/>
      <path class="arc-dash"  d="M2899.6,626.5c-56.9,-181.8,-632.7,-324.7,-1334.6,-324.7c-716.9,0,-1302.2,149,-1337.5,336.4"/>
    </svg>

    <!-- Speaker row (left panel) -->
    <img
      src="/headshot.png"
      class="headshot"
      style="view-transition-name: headshot"
      alt="Jesse Stewart"
    />
    <div class="speaker-name"  style="view-transition-name: speaker-name">JESSE STEWART</div>
    <div class="speaker-title" style="view-transition-name: speaker-title">STAFF SOFTWARE ENGINEER</div>
    <div class="speaker-url"   style="view-transition-name: speaker-url">www.JesseStewart.com</div>

    <!-- Bio text (left panel) -->
    <div class="bio">
      <slot />
    </div>

    <!-- East Lansing label (right panel) -->
    <div class="location-banner" style="view-transition-name: location-banner">EAST LANSING, MI</div>

    <!-- Map marker pin -->
    <img src="/marker.svg" class="map-marker" style="view-transition-name: map-marker" alt="" />

  </div>
</template>

<style scoped>
/* ── Slide container ── */
.about-slide {
  width: 100%;
  height: 100%;
  position: relative;
  display: flex;
  overflow: hidden;
  font-family: var(--font-sans);
}

/* ── Panels ── */
.left-panel {
  width: 66.8%;
  flex-shrink: 0;
  position: relative;
  background: white;
}

.right-panel {
  flex: 1;
  position: relative;
  z-index: 0;
}

/* City banner: 200% wide
   Left panel  → right edge anchored (shows right side of illustration)
   Right panel → left edge anchored, mirrored */
.left-panel .city-banner {
  position: absolute;
  bottom: 0;
  left: -30%;
  width: 200%;
  max-width: none;
  z-index: 1;
}

.map {
  position: absolute;
  inset: 0;
  width: 100%;
  height: 100%;
  object-fit: cover;
}

/* ── Path arc ── */
.path-arc {
  position: absolute;
  left: -35%;
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
  left: 11%; 
  top: 18%;  
  width: 102px;
  height: 102px;
  border-radius: 50%;
  object-fit: cover;
  z-index: 10;
}

.speaker-name {
  position: absolute;
  left: 24%; 
  top: 12%;  
  font-family: var(--font-display-uber);
  font-size: 32px;
  font-weight: 900;
  color: var(--color-blue);
  text-transform: uppercase;
  z-index: 10;
  white-space: nowrap;
}

.speaker-title {
  position: absolute;
  left: 24%;
  top: 26%;  
  font-family: var(--font-sans);
  font-size: 18px;
  font-weight: 700;
  color: var(--color-blue);
  text-transform: uppercase;
  letter-spacing: 1px;
  z-index: 10;
}

.speaker-url {
  position: absolute;
  left: 24%;
  top: 30.5%;  
  font-family: var(--font-mono);
  font-size: 18px;
  color: var(--color-blue);
  z-index: 10;
}

/* ── Bio text ── */
.bio {
  position: absolute;
  left: 24%;
  top: 38%;
  width: 39%;
  font-family: var(--font-display);
  font-size: 16px;
  line-height: 1.65;
  color: var(--color-blue-dark);
  z-index: 10;
}
.bio :deep(strong) { font-family: var(--font-display); font-weight: 700; }

/* ── East Lansing label ── */
.location-banner {
  position: absolute;
  left: 75%;
  bottom: 60%;
  height: 7.9%;
  background: var(--color-blue-dark);
  clip-path: polygon(3.6% 0%, 100% 0%, 96.4% 100%, 0% 100%);
  display: flex;
  align-items: center;
  text-align: center;
  padding: 0 2%;
  color: white;
  font-family: var(--font-display);
  font-style: italic;
  font-weight: 700;
  font-size: 20px;
  white-space: nowrap;
  z-index: 10;
}

/* ── Map marker ── */
.map-marker {
  position: absolute;
  left: 83.7%; 
  bottom: 45%; 
  width: 40px;
  z-index: 10;
}
</style>
