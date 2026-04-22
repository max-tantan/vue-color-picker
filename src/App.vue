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
@import url('https://fonts.googleapis.com/css2?family=JetBrains+Mono:wght@400;500;600&family=Outfit:wght@300;400;500;600;700&display=swap');

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

:root {
  --bg-dark: #0a0a0f;
  --card-bg: rgba(255, 255, 255, 0.03);
  --card-border: rgba(255, 255, 255, 0.08);
  --text-primary: #ffffff;
  --text-secondary: rgba(255, 255, 255, 0.5);
  --accent: v-bind(color);
}

.picker-container {
  min-height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
  background: var(--bg-dark);
  font-family: 'Outfit', sans-serif;
  position: relative;
  overflow: hidden;
}

.bg-orbs {
  position: fixed;
  inset: 0;
  pointer-events: none;
  z-index: 0;
}

.orb {
  position: absolute;
  border-radius: 50%;
  filter: blur(100px);
  opacity: 0.5;
  animation: float 25s ease-in-out infinite;
}

.orb-1 {
  width: 800px;
  height: 800px;
  background: linear-gradient(135deg, #ff6b6b, #feca57);
  top: -300px;
  left: -300px;
  animation-delay: 0s;
}

.orb-2 {
  width: 700px;
  height: 700px;
  background: linear-gradient(135deg, #48dbfb, #5f27cd);
  bottom: -250px;
  right: -250px;
  animation-delay: -8s;
}

.orb-3 {
  width: 600px;
  height: 600px;
  background: linear-gradient(135deg, #1dd1a1, #54a0ff);
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  animation-delay: -16s;
}

@keyframes float {
  0%, 100% { transform: translate(0, 0) scale(1); }
  25% { transform: translate(30px, -30px) scale(1.05); }
  50% { transform: translate(-20px, 20px) scale(0.95); }
  75% { transform: translate(20px, 30px) scale(1.02); }
}

.picker-card {
  position: relative;
  z-index: 1;
  width: 100%;
  max-width: 420px;
  padding: 40px 48px;
  background: var(--card-bg);
  border: 1px solid var(--card-border);
  border-radius: 28px;
  backdrop-filter: blur(20px);
  box-shadow: 
    0 0 0 1px rgba(255, 255, 255, 0.05),
    0 25px 60px rgba(0, 0, 0, 0.4),
    inset 0 1px 0 rgba(255, 255, 255, 0.05);
}

.picker-header {
  text-align: center;
  margin-bottom: 36px;
}

.logo {
  display: flex;
  justify-content: center;
  margin-bottom: 16px;
}

.title {
  font-size: 1.75rem;
  font-weight: 600;
  color: var(--text-primary);
  letter-spacing: -0.03em;
  margin-bottom: 6px;
}

.subtitle {
  font-size: 0.9rem;
  color: var(--text-secondary);
  font-weight: 400;
  letter-spacing: 0.1em;
  text-transform: uppercase;
}

.preview-section {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-bottom: 32px;
  position: relative;
}

.preview-ring {
  position: relative;
  width: 180px;
  height: 180px;
  border-radius: 50%;
  border: 3px solid;
  padding: 5px;
  box-shadow: 
    0 0 0 1px rgba(255, 255, 255, 0.1),
    0 10px 40px rgba(0, 0, 0, 0.3);
}

.color-indicator {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 160px;
  height: 160px;
  border-radius: 50%;
  opacity: 0.15;
  filter: blur(35px);
  z-index: -1;
  transition: background-color 0.25s ease;
}

.preview-inner {
  width: 100%;
  height: 100%;
  border-radius: 50%;
  transition: background-color 0.25s ease;
}

.color-indicator {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 160px;
  height: 160px;
  border-radius: 50%;
  opacity: 0.15;
  filter: blur(35px);
  z-index: -1;
  transition: background-color 0.25s ease;
}

.input-wrapper {
  margin-bottom: 32px;
  text-align: center;
  padding: 0 20px;
}

.color-input {
  width: 100%;
  height: 56px;
  border: none;
  border-radius: 14px;
  cursor: pointer;
  background: transparent;
  padding: 0;
  transition: all 0.2s ease;
}

.color-input::-webkit-color-swatch-wrapper {
  padding: 0;
}

.color-input::-webkit-color-swatch {
  border: 2px solid var(--card-border);
  border-radius: 14px;
}

.color-input:hover::-webkit-color-swatch {
  border-color: rgba(255, 255, 255, 0.2);
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.2);
}

.input-hint {
  margin-top: 12px;
  opacity: 0;
  transition: opacity 0.2s ease;
}

.color-input:hover + .input-hint {
  opacity: 1;
}

.input-hint span {
  font-size: 0.8rem;
  color: var(--text-secondary);
}

.color-values {
  display: flex;
  flex-direction: column;
  gap: 12px;
}

.value-row {
  display: flex;
  align-items: center;
  padding: 18px 20px;
  background: rgba(255, 255, 255, 0.02);
  border: 1px solid var(--card-border);
  border-radius: 14px;
  cursor: pointer;
  transition: all 0.25s ease;
  position: relative;
  overflow: hidden;
}

.value-row::before {
  content: '';
  position: absolute;
  inset: 0;
  background: linear-gradient(90deg, transparent, rgba(255,255,255,0.03), transparent);
  transform: translateX(-100%);
  transition: transform 0.5s ease;
}

.value-row:hover::before {
  transform: translateX(100%);
}

.value-row:hover {
  background: rgba(255, 255, 255, 0.04);
  border-color: rgba(255, 255, 255, 0.15);
  transform: translateX(6px);
}

.value-row:active {
  transform: translateX(3px);
}

.value-row.copied {
  border-color: #22c55e;
  background: rgba(34, 197, 94, 0.08);
}

.value-row.copied::before {
  display: none;
}

.value-label {
  font-size: 0.8rem;
  font-weight: 600;
  color: var(--text-secondary);
  width: 40px;
  text-transform: uppercase;
  letter-spacing: 0.08em;
}

.value-content {
  flex: 1;
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.value-text {
  font-family: 'JetBrains Mono', monospace;
  font-size: 1rem;
  font-weight: 500;
  color: var(--text-primary);
  letter-spacing: 0.02em;
}

.copy-icon {
  color: var(--text-secondary);
  opacity: 0;
  transform: translateX(6px);
  transition: all 0.25s ease;
}

.value-row:hover .copy-icon {
  opacity: 1;
  transform: translateX(0);
}

.copied-badge {
  position: absolute;
  top: 50%;
  right: 20px;
  transform: translateY(-50%);
  font-size: 0.8rem;
  font-weight: 600;
  color: #22c55e;
  opacity: 0;
  transition: all 0.25s ease;
}

.value-row.copied .copied-badge {
  opacity: 1;
}

.picker-footer {
  margin-top: 32px;
  text-align: center;
  padding-top: 20px;
  border-top: 1px solid var(--card-border);
}

.footer-text {
  font-size: 0.8rem;
  color: var(--text-secondary);
  letter-spacing: 0.05em;
}
</style>