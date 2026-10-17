<script setup lang="ts">
import { provide, ref, watch } from 'vue'
import { BackTop } from '@/components/ui/back-top'
import { useAppStore } from '@/stores/app'
import { buildGlassThemeTokens, buildTransparentTokens } from '@/utils/glassTheme'

const appStore = useAppStore()

const isScrolled = ref(false)
provide('isScrolled', isScrolled)

watch(
  () => appStore.resolvedThemeMode,
  (mode) => {
    const root = document.documentElement
    root.classList.toggle('dark', mode === 'dark')
    root.classList.toggle('transparent', mode === 'transparent')
    root.style.colorScheme = mode === 'light' ? 'light' : 'dark'
  },
  { immediate: true },
)

watch(
  () => [appStore.glassColorPreset, appStore.glassCustomColors] as const,
  ([preset, customColors]) => {
    const tokens = buildGlassThemeTokens(preset, customColors)
    const root = document.documentElement
    root.style.setProperty('--glass-light-card', tokens.lightCard)
    root.style.setProperty('--glass-light-card-hover', tokens.lightCardHover)
    root.style.setProperty('--glass-light-control', tokens.lightControl)
    root.style.setProperty('--glass-light-header', tokens.lightHeader)
    root.style.setProperty('--glass-light-text', tokens.lightText)
    root.style.setProperty('--glass-light-muted-text', tokens.lightMutedText)
    root.style.setProperty('--glass-light-border', tokens.lightBorder)
    root.style.setProperty('--glass-light-shadow', tokens.lightShadow)
    root.style.setProperty('--glass-dark-card', tokens.darkCard)
    root.style.setProperty('--glass-dark-card-hover', tokens.darkCardHover)
    root.style.setProperty('--glass-dark-control', tokens.darkControl)
    root.style.setProperty('--glass-dark-header', tokens.darkHeader)
    root.style.setProperty('--glass-dark-text', tokens.darkText)
    root.style.setProperty('--glass-dark-muted-text', tokens.darkMutedText)
    root.style.setProperty('--glass-dark-border', tokens.darkBorder)
    root.style.setProperty('--glass-dark-shadow', tokens.darkShadow)
  },
  { immediate: true, deep: true },
)

watch(
  () => [appStore.transparentCardOpacity, appStore.transparentTextShadow] as const,
  ([opacity, textShadow]) => {
    const tokens = buildTransparentTokens(opacity)
    const root = document.documentElement
    root.style.setProperty('--glass-transparent-card', tokens.card)
    root.style.setProperty('--glass-transparent-card-hover', tokens.cardHover)
    root.style.setProperty('--glass-transparent-card-strong', tokens.cardStrong)
    root.style.setProperty('--glass-transparent-control', tokens.control)
    root.style.setProperty('--glass-transparent-header', tokens.header)
    root.style.setProperty('--glass-transparent-text', tokens.text)
    root.style.setProperty('--glass-transparent-muted-text', tokens.mutedText)
    root.style.setProperty('--glass-transparent-border', tokens.border)
    root.style.setProperty('--glass-transparent-shadow', tokens.shadow)
    root.style.setProperty(
      '--glass-transparent-text-shadow',
      textShadow ? '0 1px 3px rgb(0 0 0 / 0.8), 0 0 10px rgb(0 0 0 / 0.45)' : 'none',
    )
  },
  { immediate: true },
)

watch(
  () => appStore.colorVisionFriendly,
  (enabled) => {
    document.documentElement.classList.toggle('color-vision-friendly', enabled)
  },
  { immediate: true },
)
</script>

<template>
  <slot />
  <BackTop :visibility-height="320" @scrolled="isScrolled = $event" />
</template>
