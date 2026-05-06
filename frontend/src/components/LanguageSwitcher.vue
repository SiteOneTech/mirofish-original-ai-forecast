<template>
  <div class="lang-and-theme">
    <div class="language-switcher" ref="switcherRef">
      <button class="switcher-trigger" @click="toggleDropdown">
        {{ currentLabel }}
        <span class="caret">{{ open ? '▲' : '▼' }}</span>
      </button>
      <ul v-if="open" class="switcher-dropdown">
        <li
          v-for="loc in availableLocales"
          :key="loc.key"
          class="switcher-option"
          :class="{ active: loc.key === locale }"
          @click="switchLocale(loc.key)"
        >
          {{ loc.label }}
        </li>
      </ul>
    </div>

    <button
      class="theme-toggle"
      @click="toggleDark"
      :title="isDark ? $t('nav.toggleLightMode') : $t('nav.toggleDarkMode')"
    >
      <span v-if="isDark">☀</span>
      <span v-else>☾</span>
    </button>
  </div>
</template>

<script setup>
import { ref, computed, onMounted, onUnmounted } from 'vue'
import { useI18n } from 'vue-i18n'
import { availableLocales } from '@/i18n/index.js'

const { locale } = useI18n()
const open = ref(false)
const switcherRef = ref(null)
const isDark = ref(false)

const currentLabel = computed(() => {
  const found = availableLocales.find(l => l.key === locale.value)
  return found ? found.label : locale.value
})

const toggleDropdown = () => {
  open.value = !open.value
}

const switchLocale = (key) => {
  locale.value = key
  localStorage.setItem('locale', key)
  document.documentElement.lang = key
  open.value = false
}

const toggleDark = () => {
  isDark.value = !isDark.value
  document.documentElement.classList.toggle('dark', isDark.value)
  localStorage.setItem('theme', isDark.value ? 'dark' : 'light')
}

const onClickOutside = (e) => {
  if (switcherRef.value && !switcherRef.value.contains(e.target)) {
    open.value = false
  }
}

onMounted(() => {
  document.addEventListener('click', onClickOutside)
  document.documentElement.lang = locale.value
  // Restore saved theme preference
  const saved = localStorage.getItem('theme')
  if (saved === 'dark' || (!saved && window.matchMedia('(prefers-color-scheme: dark)').matches)) {
    isDark.value = true
    document.documentElement.classList.add('dark')
  }
})

onUnmounted(() => {
  document.removeEventListener('click', onClickOutside)
})
</script>

<style scoped>
.lang-and-theme {
  display: flex;
  align-items: center;
  gap: 6px;
}

.language-switcher {
  position: relative;
  display: inline-block;
  font-family: 'JetBrains Mono', monospace;
}

/* Light theme (default - for white header backgrounds) */
.switcher-trigger {
  background: transparent;
  color: #333;
  border: 1px solid #CCC;
  padding: 4px 12px;
  font-family: 'JetBrains Mono', monospace;
  font-size: 0.8rem;
  cursor: pointer;
  display: flex;
  align-items: center;
  gap: 6px;
  transition: border-color 0.2s, opacity 0.2s;
}

.switcher-trigger:hover {
  border-color: #999;
}

.caret {
  font-size: 0.6rem;
}

.switcher-dropdown {
  position: absolute;
  top: 100%;
  right: 0;
  margin-top: 4px;
  background: #FFFFFF;
  border: 1px solid #DDD;
  list-style: none;
  padding: 4px 0;
  min-width: 100%;
  z-index: 1000;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
}

.switcher-option {
  padding: 6px 12px;
  font-size: 0.8rem;
  color: #333;
  cursor: pointer;
  white-space: nowrap;
  transition: background 0.15s;
}

.switcher-option:hover {
  background: #F0F0F0;
}

.switcher-option.active {
  color: var(--orange, #FF4500);
}

.theme-toggle {
  background: transparent;
  color: #333;
  border: 1px solid #CCC;
  padding: 4px 8px;
  font-family: 'JetBrains Mono', monospace;
  font-size: 0.9rem;
  cursor: pointer;
  line-height: 1;
  transition: border-color 0.2s, background 0.2s;
}

.theme-toggle:hover {
  border-color: #999;
  background: rgba(0, 0, 0, 0.04);
}
</style>
