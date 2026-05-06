<template>
  <router-view />
</template>

<script setup>
// 使用 Vue Router 来管理页面
</script>

<style>
/* 全局样式重置 */
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

/* ── Light / Dark theme CSS variables ── */
:root {
  --bg: #ffffff;
  --bg-secondary: #f9fafb;
  --bg-elevated: #f3f4f6;
  --text-primary: #000000;
  --text-secondary: #374151;
  --text-muted: #6b7280;
  --border: #e5e7eb;
  --border-strong: #d1d5db;
  --scrollbar-track: #f1f1f1;
  --scrollbar-thumb: #000000;
  --scrollbar-thumb-hover: #333333;
  --input-bg: #ffffff;
  --input-border: #d1d5db;
  --navbar-bg: #ffffff;
  --card-bg: #ffffff;
  --shadow: rgba(0, 0, 0, 0.08);
}

html.dark {
  --bg: #111827;
  --bg-secondary: #1f2937;
  --bg-elevated: #374151;
  --text-primary: #f9fafb;
  --text-secondary: #d1d5db;
  --text-muted: #9ca3af;
  --border: #374151;
  --border-strong: #4b5563;
  --scrollbar-track: #1f2937;
  --scrollbar-thumb: #6b7280;
  --scrollbar-thumb-hover: #9ca3af;
  --input-bg: #1f2937;
  --input-border: #4b5563;
  --navbar-bg: #111827;
  --card-bg: #1f2937;
  --shadow: rgba(0, 0, 0, 0.4);
}

html.dark body {
  background-color: var(--bg);
  color: var(--text-primary);
}

#app {
  font-family: 'JetBrains Mono', 'Space Grotesk', 'Noto Sans SC', monospace;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: var(--text-primary);
  background-color: var(--bg);
  transition: background-color 0.2s ease, color 0.2s ease;
}

/* Home page dark mode */
html.dark .home-container {
  background: var(--bg);
  color: var(--text-primary);
}

html.dark .navbar {
  background: var(--navbar-bg);
  border-bottom-color: var(--border);
}

html.dark .nav-brand,
html.dark .github-link {
  color: var(--text-primary) !important;
}

html.dark .console-box {
  background: var(--bg-secondary);
  border-color: var(--border);
}

html.dark .upload-zone {
  background: var(--bg-elevated);
  border-color: var(--border-strong);
  color: var(--text-secondary);
}

html.dark .code-input {
  background: var(--input-bg) !important;
  border-color: var(--input-border) !important;
  color: var(--text-primary) !important;
}

html.dark .code-input::placeholder {
  color: var(--text-muted) !important;
}

html.dark .left-panel,
html.dark .right-panel {
  background: var(--bg-secondary);
  border-color: var(--border);
}

html.dark .panel-header {
  color: var(--text-muted);
}

html.dark .section-title,
html.dark .main-title,
html.dark .hero-title {
  color: var(--text-primary);
}

html.dark .metric-card {
  background: var(--bg-elevated);
  border-color: var(--border);
  color: var(--text-secondary);
}

html.dark .workflow-item {
  border-color: var(--border);
}

html.dark .step-desc,
html.dark .section-desc {
  color: var(--text-muted);
}

/* Report panel dark mode */
html.dark .report-panel {
  background: var(--bg);
}

html.dark .left-panel.report-style {
  background: var(--bg);
  border-right-color: var(--border);
}

html.dark .report-header-block {
  border-bottom-color: var(--border);
}

html.dark .main-title,
html.dark .sub-title {
  color: var(--text-primary);
}

html.dark .report-section-item {
  border-color: var(--border);
}

html.dark .report-section-item .section-title {
  color: var(--text-primary);
}

html.dark .generated-content {
  color: var(--text-secondary);
}

/* Simulation panel dark mode */
html.dark .simulation-panel,
html.dark .control-bar {
  background: var(--bg);
  border-color: var(--border);
}

html.dark .platform-status {
  background: var(--bg-secondary);
  border-color: var(--border);
  color: var(--text-secondary);
}

html.dark .timeline-item {
  background: var(--bg-secondary);
  border-color: var(--border);
}

/* LanguageSwitcher dark mode */
html.dark .switcher-trigger {
  color: var(--text-secondary);
  border-color: var(--border-strong);
}

html.dark .switcher-dropdown {
  background: var(--bg-secondary);
  border-color: var(--border-strong);
}

html.dark .switcher-option {
  color: var(--text-secondary);
}

html.dark .switcher-option:hover {
  background: var(--bg-elevated);
}

html.dark .theme-toggle {
  color: var(--text-secondary);
  border-color: var(--border-strong);
}

html.dark .theme-toggle:hover {
  background: var(--bg-elevated);
}

/* 滚动条样式 */
::-webkit-scrollbar {
  width: 8px;
  height: 8px;
}

::-webkit-scrollbar-track {
  background: var(--scrollbar-track);
}

::-webkit-scrollbar-thumb {
  background: var(--scrollbar-thumb);
}

::-webkit-scrollbar-thumb:hover {
  background: var(--scrollbar-thumb-hover);
}

/* 全局按钮样式 */
button {
  font-family: inherit;
}
</style>
