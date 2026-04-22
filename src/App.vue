<template>
  <div class="picker-container">
    <div class="bg-orbs">
      <div class="orb orb-1"></div>
      <div class="orb orb-2"></div>
      <div class="orb orb-3"></div>
    </div>
    
    <div class="picker-card">
      <header class="picker-header">
        <div class="logo">
          <svg width="28" height="28" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
            <circle cx="12" cy="12" r="10" :stroke="textColor" stroke-width="2"/>
            <circle cx="12" cy="12" r="6" :fill="color"/>
          </svg>
        </div>
        <h1 class="title">Color Picker</h1>
        <p class="subtitle">Pick & Copy</p>
      </header>

      <div class="preview-section">
        <div class="preview-ring" :style="{ borderColor: color }">
          <div class="preview-inner" :style="{ backgroundColor: color }"></div>
        </div>
        <div class="color-indicator" :style="{ backgroundColor: color }"></div>
      </div>

      <div class="input-wrapper">
        <input
          type="color"
          v-model="color"
          class="color-input"
        />
        <div class="input-hint">
          <span>Click to open picker</span>
        </div>
      </div>

      <div class="color-values">
        <div class="value-row" @click="copy(color)" :class="{ copied: copiedField === 'hex' }">
          <div class="value-label">HEX</div>
          <div class="value-content">
            <span class="value-text">{{ color.toUpperCase() }}</span>
            <svg class="copy-icon" width="16" height="16" viewBox="0 0 24 24" fill="none">
              <rect x="9" y="9" width="11" height="11" rx="2" stroke="currentColor" stroke-width="2"/>
              <path d="M5 15H4C2.89543 15 2 14.1046 2 13V4C2 2.89543 2.89543 2 4 2H13C14.1046 2 15 2.89543 15 4V5" stroke="currentColor" stroke-width="2"/>
            </svg>
          </div>
          <div class="copied-badge">Copied!</div>
        </div>

        <div class="value-row" @click="copy(rgb)" :class="{ copied: copiedField === 'rgb' }">
          <div class="value-label">RGB</div>
          <div class="value-content">
            <span class="value-text">{{ rgb }}</span>
            <svg class="copy-icon" width="16" height="16" viewBox="0 0 24 24" fill="none">
              <rect x="9" y="9" width="11" height="11" rx="2" stroke="currentColor" stroke-width="2"/>
              <path d="M5 15H4C2.89543 15 2 14.1046 2 13V4C2 2.89543 2.89543 2 4 2H13C14.1046 2 15 2.89543 15 4V5" stroke="currentColor" stroke-width="2"/>
            </svg>
          </div>
          <div class="copied-badge">Copied!</div>
        </div>

        <div class="value-row" @click="copy(hsl)" :class="{ copied: copiedField === 'hsl' }">
          <div class="value-label">HSL</div>
          <div class="value-content">
            <span class="value-text">{{ hsl }}</span>
            <svg class="copy-icon" width="16" height="16" viewBox="0 0 24 24" fill="none">
              <rect x="9" y="9" width="11" height="11" rx="2" stroke="currentColor" stroke-width="2"/>
              <path d="M5 15H4C2.89543 15 2 14.1046 2 13V4C2 2.89543 2.89543 2 4 2H13C14.1046 2 15 2.89543 15 4V5" stroke="currentColor" stroke-width="2"/>
            </svg>
          </div>
          <div class="copied-badge">Copied!</div>
        </div>
      </div>

      <footer class="picker-footer">
        <p class="footer-text">Click any value to copy</p>
      </footer>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from "vue";

const color = ref("#6366f1");
const copiedField = ref(null);

const textColor = computed(() => {
  const r = parseInt(color.value.slice(1, 3), 16);
  const g = parseInt(color.value.slice(3, 5), 16);
  const b = parseInt(color.value.slice(5, 7), 16);
  const luminance = (0.299 * r + 0.587 * g + 0.114 * b) / 255;
  return luminance > 0.5 ? '#1a1a2e' : '#ffffff';
});

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
  }, 1200);
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
  filter: blur(80px);
  opacity: 0.6;
  animation: float 20s ease-in-out infinite;
}

.orb-1 {
  width: 600px;
  height: 600px;
  background: linear-gradient(135deg, #ff6b6b, #feca57);
  top: -200px;
  left: -200px;
  animation-delay: 0s;
}

.orb-2 {
  width: 500px;
  height: 500px;
  background: linear-gradient(135deg, #48dbfb, #5f27cd);
  bottom: -150px;
  right: -150px;
  animation-delay: -7s;
}

.orb-3 {
  width: 400px;
  height: 400px;
  background: linear-gradient(135deg, #1dd1a1, #54a0ff);
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  animation-delay: -14s;
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
  max-width: 380px;
  padding: 32px;
  background: var(--card-bg);
  border: 1px solid var(--card-border);
  border-radius: 24px;
  backdrop-filter: blur(20px);
  box-shadow: 
    0 0 0 1px rgba(255, 255, 255, 0.05),
    0 20px 50px rgba(0, 0, 0, 0.3),
    inset 0 1px 0 rgba(255, 255, 255, 0.05);
}

.picker-header {
  text-align: center;
  margin-bottom: 28px;
}

.logo {
  display: flex;
  justify-content: center;
  margin-bottom: 12px;
}

.title {
  font-size: 1.5rem;
  font-weight: 600;
  color: var(--text-primary);
  letter-spacing: -0.02em;
  margin-bottom: 4px;
}

.subtitle {
  font-size: 0.875rem;
  color: var(--text-secondary);
  font-weight: 400;
}

.preview-section {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-bottom: 24px;
}

.preview-ring {
  width: 140px;
  height: 140px;
  border-radius: 50%;
  border: 3px solid;
  padding: 4px;
  position: relative;
}

.preview-inner {
  width: 100%;
  height: 100%;
  border-radius: 50%;
  transition: background-color 0.2s ease;
}

.color-indicator {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 120px;
  height: 120px;
  border-radius: 50%;
  opacity: 0.1;
  filter: blur(30px);
  z-index: -1;
  transition: background-color 0.2s ease;
}

.input-wrapper {
  margin-bottom: 24px;
  text-align: center;
}

.color-input {
  width: 100%;
  height: 48px;
  border: none;
  border-radius: 12px;
  cursor: pointer;
  background: transparent;
  padding: 0;
}

.color-input::-webkit-color-swatch-wrapper {
  padding: 0;
}

.color-input::-webkit-color-swatch {
  border: 2px solid var(--card-border);
  border-radius: 12px;
}

.input-hint {
  margin-top: 8px;
}

.input-hint span {
  font-size: 0.75rem;
  color: var(--text-secondary);
}

.color-values {
  display: flex;
  flex-direction: column;
  gap: 10px;
}

.value-row {
  display: flex;
  align-items: center;
  padding: 14px 16px;
  background: rgba(255, 255, 255, 0.02);
  border: 1px solid var(--card-border);
  border-radius: 12px;
  cursor: pointer;
  transition: all 0.2s ease;
  position: relative;
  overflow: hidden;
}

.value-row:hover {
  background: rgba(255, 255, 255, 0.05);
  border-color: rgba(255, 255, 255, 0.15);
  transform: translateX(4px);
}

.value-row:active {
  transform: translateX(2px);
}

.value-row.copied {
  border-color: #22c55e;
  background: rgba(34, 197, 94, 0.1);
}

.value-label {
  font-size: 0.75rem;
  font-weight: 600;
  color: var(--text-secondary);
  width: 36px;
  text-transform: uppercase;
  letter-spacing: 0.05em;
}

.value-content {
  flex: 1;
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.value-text {
  font-family: 'JetBrains Mono', monospace;
  font-size: 0.9rem;
  font-weight: 500;
  color: var(--text-primary);
}

.copy-icon {
  color: var(--text-secondary);
  opacity: 0;
  transform: translateX(4px);
  transition: all 0.2s ease;
}

.value-row:hover .copy-icon {
  opacity: 1;
  transform: translateX(0);
}

.copied-badge {
  position: absolute;
  top: 50%;
  right: 16px;
  transform: translateY(-50%);
  font-size: 0.75rem;
  font-weight: 600;
  color: #22c55e;
  opacity: 0;
  transition: all 0.2s ease;
}

.value-row.copied .copied-badge {
  opacity: 1;
}

.picker-footer {
  margin-top: 24px;
  text-align: center;
}

.footer-text {
  font-size: 0.75rem;
  color: var(--text-secondary);
}
</style>