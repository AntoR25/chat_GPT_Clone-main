<template>
  <div>
    <NuxtPage />
  </div>
</template>

<script setup>
// Configuration SEO de base pour l'ensemble de l'application
useHead({
  title: 'ChatGPT Clone - Assistant',
  meta: [
    { name: 'description', content: 'Clone de ChatGPT utilisant Nuxt 3 & Tailwind CSS' }
  ],
  bodyAttrs: {
    class: 'bg-[#343541]' // Couleur de fond par défaut pour éviter le flash blanc au chargement
  }
})
</script>