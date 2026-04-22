<template>
  <div class="picker-container">
    <div class="bg-grid"></div>
    <div class="bg-glow glow-1"></div>
    <div class="bg-glow glow-2"></div>
    <div class="bg-glow glow-3"></div>
    
    <div class="picker-card">
      <header class="picker-header">
        <div class="logo-mark">
          <svg width="36" height="36" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
            <circle cx="12" cy="12" r="10" stroke="rgba(255,255,255,0.3)" stroke-width="1.5"/>
            <circle cx="12" cy="12" r="7" :fill="color"/>
            <circle cx="12" cy="12" r="4" fill="rgba(0,0,0,0.2)"/>
          </svg>
        </div>
        <h1 class="title">Color Picker</h1>
        <div class="separator"></div>
      </header>

      <div class="preview-section">
        <div class="preview-outer" :style="{ '--accent': color }">
          <div class="preview-glow" :style="{ backgroundColor: color }"></div>
          <div class="preview-ring" :style="{ borderColor: color }">
            <div class="preview-inner" :style="{ backgroundColor: color }"></div>
          </div>
        </div>
        <div class="preview-label">Current Color</div>
      </div>

      <div class="input-section">
        <label class="input-label">Pick a color</label>
        <div class="input-wrapper">
          <input
            type="color"
            v-model="color"
            class="color-input"
          />
        </div>
      </div>

      <div class="values-section">
        <label class="values-label">Color Values</label>
        
        <div class="value-row" @click="copy(color)" :class="{ copied: copiedField === 'hex' }">
          <div class="value-type">HEX</div>
          <div class="value-data">
            <span class="value-text">{{ color.toUpperCase() }}</span>
            <svg class="value-icon" width="18" height="18" viewBox="0 0 24 24" fill="none">
              <rect x="9" y="9" width="11" height="11" rx="2" stroke="currentColor" stroke-width="2"/>
              <path d="M5 15H4C2.89543 15 2 14.1046 2 13V4C2 2.89543 2.89543 2 4 2H13C14.1046 2 15 2.89543 15 4V5" stroke="currentColor" stroke-width="2"/>
            </svg>
          </div>
          <div class="value-check">
            <svg width="16" height="16" viewBox="0 0 24 24" fill="none">
              <path d="M5 12l5 5L20 7" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round"/>
            </svg>
          </div>
        </div>

        <div class="value-row" @click="copy(rgb)" :class="{ copied: copiedField === 'rgb' }">
          <div class="value-type">RGB</div>
          <div class="value-data">
            <span class="value-text">{{ rgb }}</span>
            <svg class="value-icon" width="18" height="18" viewBox="0 0 24 24" fill="none">
              <rect x="9" y="9" width="11" height="11" rx="2" stroke="currentColor" stroke-width="2"/>
              <path d="M5 15H4C2.89543 15 2 14.1046 2 13V4C2 2.89543 2.89543 2 4 2H13C14.1046 2 15 2.89543 15 4V5" stroke="currentColor" stroke-width="2"/>
            </svg>
          </div>
          <div class="value-check">
            <svg width="16" height="16" viewBox="0 0 24 24" fill="none">
              <path d="M5 12l5 5L20 7" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round"/>
            </svg>
          </div>
        </div>

        <div class="value-row" @click="copy(hsl)" :class="{ copied: copiedField === 'hsl' }">
          <div class="value-type">HSL</div>
          <div class="value-data">
            <span class="value-text">{{ hsl }}</span>
            <svg class="value-icon" width="18" height="18" viewBox="0 0 24 24" fill="none">
              <rect x="9" y="9" width="11" height="11" rx="2" stroke="currentColor" stroke-width="2"/>
              <path d="M5 15H4C2.89543 15 2 14.1046 2 13V4C2 2.89543 2.89543 2 4 2H13C14.1046 2 15 2.89543 15 4V5" stroke="currentColor" stroke-width="2"/>
            </svg>
          </div>
          <div class="value-check">
            <svg width="16" height="16" viewBox="0 0 24 24" fill="none">
              <path d="M5 12l5 5L20 7" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round"/>
            </svg>
          </div>
        </div>
      </div>

      <footer class="picker-footer">
        <div class="footer-hint">
          <span>Click any row to copy to clipboard</span>
        </div>
      </footer>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from "vue";

const color = ref("#6366f1");
const copiedField = ref(null);

const rgb = computed(() => {
  const r = parseInt(color.value.slice(1, 3), 16);
  const g = parseInt(color.value.slice(3, 5), 16);
  const b = parseInt(color.value.slice(5, 7), 16);
  return `rgb(${r}, ${g}, ${b})`;
});

const hsl = computed(() => {
  let r = parseInt(color.value.slice(1, 3), 16) / 255;
  let g = parseInt(color.value.slice(3, 5), 16) / 255;
  let b = parseInt(color.value.slice(5, 7), 16) / 255;

  const max = Math.max(r, g, b);
  const min = Math.min(r, g, b);
  let h, s, l = (max + min) / 2;

  if (max === min) {
    h = s = 0;
  } else {
    const d = max - min;
    s = l > 0.5 ? d / (2 - max - min) : d / (max + min);
    switch (max) {
      case r: h = ((g - b) / d + (g < b ? 6 : 0)) / 6; break;
      case g: h = ((b - r) / d + 2) / 6; break;
      case b: h = ((r - g) / d + 4) / 6; break;
    }
  }

  return `hsl(${Math.round(h * 360)}, ${Math.round(s * 100)}%, ${Math.round(l * 100)}%)`;
});

const copy = async (text, field) => {
  await navigator.clipboard.writeText(text);
  copiedField.value = field;

  setTimeout(() => {
    copiedField.value = null;
  }, 1500);
};
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=JetBrains+Mono:wght@400;500;600&family=Space+Grotesk:wght@400;500;600;700&display=swap');

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

.picker-container {
  min-height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
  background: #050507;
  font-family: 'Space Grotesk', sans-serif;
  position: relative;
  overflow: hidden;
}

.bg-grid {
  position: fixed;
  inset: 0;
  background-image: 
    linear-gradient(rgba(255,255,255,0.02) 1px, transparent 1px),
    linear-gradient(90deg, rgba(255,255,255,0.02) 1px, transparent 1px);
  background-size: 60px 60px;
  pointer-events: none;
}

.bg-glow {
  position: fixed;
  border-radius: 50%;
  filter: blur(120px);
  pointer-events: none;
}

.glow-1 {
  width: 900px;
  height: 900px;
  background: rgba(99, 102, 241, 0.15);
  top: -400px;
  left: -200px;
}

.glow-2 {
  width: 700px;
  height: 700px;
  background: rgba(236, 72, 153, 0.1);
  bottom: -300px;
  right: -200px;
}

.glow-3 {
  width: 500px;
  height: 500px;
  background: rgba(34, 197, 94, 0.08);
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}

.picker-card {
  position: relative;
  z-index: 1;
  width: 100%;
  max-width: 440px;
  padding: 48px;
  background: rgba(12, 12, 15, 0.8);
  border: 1px solid rgba(255, 255, 255, 0.06);
  border-radius: 32px;
  box-shadow: 
    0 0 0 1px rgba(255, 255, 255, 0.03),
    0 40px 80px rgba(0, 0, 0, 0.5);
}

.picker-header {
  text-align: center;
  margin-bottom: 40px;
}

.logo-mark {
  display: flex;
  justify-content: center;
  margin-bottom: 20px;
}

.title {
  font-size: 1.5rem;
  font-weight: 600;
  color: #fff;
  letter-spacing: -0.02em;
}

.separator {
  width: 60px;
  height: 2px;
  background: linear-gradient(90deg, transparent, rgba(255,255,255,0.2), transparent);
  margin: 20px auto 0;
}

.preview-section {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-bottom: 36px;
}

.preview-outer {
  position: relative;
  width: 200px;
  height: 200px;
}

.preview-glow {
  position: absolute;
  inset: 0;
  border-radius: 50%;
  opacity: 0.15;
  filter: blur(40px);
  transition: background-color 0.3s ease;
}

.preview-ring {
  position: absolute;
  inset: 0;
  border-radius: 50%;
  border: 3px solid;
  padding: 6px;
}

.preview-inner {
  width: 100%;
  height: 100%;
  border-radius: 50%;
  transition: background-color 0.3s ease;
  box-shadow: inset 0 2px 4px rgba(0,0,0,0.2);
}

.preview-label {
  margin-top: 16px;
  font-size: 0.8rem;
  color: rgba(255,255,255,0.4);
  letter-spacing: 0.1em;
  text-transform: uppercase;
}

.input-section {
  margin-bottom: 36px;
}

.input-label {
  display: block;
  font-size: 0.8rem;
  color: rgba(255,255,255,0.4);
  letter-spacing: 0.08em;
  text-transform: uppercase;
  margin-bottom: 12px;
}

.input-wrapper {
  width: 100%;
}

.color-input {
  width: 100%;
  height: 60px;
  border: none;
  border-radius: 16px;
  cursor: pointer;
  background: transparent;
  padding: 0;
}

.color-input::-webkit-color-swatch-wrapper {
  padding: 0;
}

.color-input::-webkit-color-swatch {
  border: 1px solid rgba(255,255,255,0.1);
  border-radius: 16px;
}

.values-section {
  margin-bottom: 36px;
}

.values-label {
  display: block;
  font-size: 0.8rem;
  color: rgba(255,255,255,0.4);
  letter-spacing: 0.08em;
  text-transform: uppercase;
  margin-bottom: 16px;
}

.value-row {
  display: flex;
  align-items: center;
  padding: 18px 20px;
  background: rgba(255, 255, 255, 0.02);
  border: 1px solid rgba(255, 255, 255, 0.06);
  border-radius: 16px;
  cursor: pointer;
  transition: all 0.25s ease;
  position: relative;
  overflow: hidden;
}

.value-row:hover {
  background: rgba(255, 255, 255, 0.04);
  border-color: rgba(255, 255, 255, 0.12);
  transform: translateX(4px);
}

.value-row:active {
  transform: translateX(2px);
}

.value-row.copied {
  border-color: #22c55e;
  background: rgba(34, 197, 94, 0.08);
}

.value-type {
  font-size: 0.75rem;
  font-weight: 600;
  color: rgba(255,255,255,0.35);
  width: 36px;
  text-transform: uppercase;
  letter-spacing: 0.1em;
}

.value-data {
  flex: 1;
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.value-text {
  font-family: 'JetBrains Mono', monospace;
  font-size: 0.95rem;
  font-weight: 500;
  color: #fff;
}

.value-icon {
  color: rgba(255,255,255,0.25);
  transition: all 0.25s ease;
}

.value-row:hover .value-icon {
  color: rgba(255,255,255,0.5);
  transform: scale(1.05);
}

.value-check {
  position: absolute;
  right: 18px;
  color: #22c55e;
  opacity: 0;
  transform: scale(0.5);
  transition: all 0.25s ease;
}

.value-row.copied .value-check {
  opacity: 1;
  transform: scale(1);
}

.picker-footer {
  text-align: center;
  padding-top: 24px;
  border-top: 1px solid rgba(255,255,255,0.04);
}

.footer-hint span {
  font-size: 0.8rem;
  color: rgba(255,255,255,0.3);
}
</style>