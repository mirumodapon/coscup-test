<script setup lang="ts">
import type { Sponsor } from '#loaders/sponsor.data'
import { marked } from 'marked'
import { ref } from 'vue'

defineProps<{ sponsor: Sponsor }>()

const expanded = ref(false)

function toggleExpand() {
  expanded.value = !expanded.value
}

const parseMarkdown = (text: string) => marked(text || '')
</script>

<template>
  <div
    class="sponsor-card"
    @click="toggleExpand"
  >
    <a
      class="sponsor-link"
      :href="sponsor.link"
      rel="noopener noreferrer"
      target="_blank"
    >
      🔗 Visit Website
    </a>
    <div class="sponsor-image-wrapper">
      <img
        :alt="sponsor['name:en']"
        class="sponsor-image"
        :src="sponsor.image"
      >
    </div>

    <transition name="fade">
      <div
        v-if="expanded"
        class="sponsor-info"
      >
        <h3 class="sponsor-name">
          {{ sponsor['name:en'] }}
        </h3>
        <p v-html="parseMarkdown(sponsor['intro:en'])" />
      </div>
    </transition>
    <span
      v-if="sponsor.type === '3'"
      class="badge"
    >Collaborated for a total of {{ sponsor.times }} years</span>
    <span
      v-else-if="sponsor.type === '2'"
      class="badge"
    >Sponsored for a total of {{ sponsor.times }} years</span>
    <span
      v-else-if="sponsor.type === '1'"
      class="badge"
    >Sponsored for {{ sponsor.times }} consecutive years</span>
  </div>
</template>

<style scoped>
.sponsor-card {
  position: relative;
  text-align: center;
  padding: 15px;
  border-radius: 8px;
  background: var(--vp-color-sponsor-card-bg);
  border: 1px solid var(--vp-color-sponsor-card-border);
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  cursor: pointer;
  transition: transform 0.2s;
}

.sponsor-card:hover {
  transform: translateY(-4px);
}

/* Right-top link button */
.sponsor-link {
  position: absolute;
  top: 10px;
  right: 10px;
  font-size: 0.85rem;
  text-decoration: none;
  background: var(--vp-color-sponsor-bg, #fff);
  color: var(--vp-color-sponsor-link);
  padding: 4px 8px;
  border-radius: 4px;
  border: 1px solid var(--vp-color-sponsor-link);
  display: flex;
  align-items: center;
  gap: 4px;
  transition: all 0.2s ease;
  z-index: 1;
}

.sponsor-link:hover {
  background: var(--vp-color-sponsor-link);
  color: white;
}

.sponsor-image-wrapper {
  width: 100%;
  max-width: 400px;
  aspect-ratio: 3 / 2;
  margin: 0 auto;
  overflow: hidden;
  border-radius: 8px;
}

.sponsor-image {
  width: 100%;
  height: 100%;
  object-fit: cover;
  display: block;
}

.sponsor-name {
  margin-top: 10px;
  font-size: 1.1rem;
  color: var(--vp-color-sponsor-text);
  font-weight: bold;
}

.sponsor-info {
  text-align: left;
  margin-top: 10px;
  font-size: 0.9rem;
  color: var(--vp-color-sponsor-text);
}

/* Transition */
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.3s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}

.badge {
  width: 185px;
  position: absolute;
  top: 10px;
  left: 10px;
  padding: 4px 8px;
  background: var(--vp-c-brand-3);
  color: white;
  border-radius: 4px;
  font-size: 0.8rem;
  font-weight: bold;
  transform: translateY(70%) translateX(-15%) rotate(-30deg);
  font-size: 0.6em;
}
</style>
