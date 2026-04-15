<template>
  <div>
    <!-- NuxtPage là component đặc biệt để hiển thị nội dung của file trong thư mục pages -->
    <NuxtPage />
  </div>
</template>

<script setup>
// Cấu hình SEO cơ bản cho toàn app
useHead({
  title: 'ChatGPT Clone',
  meta: [
    { name: 'description', content: 'ChatGPT Clone using Nuxt 3 & Tailwind CSS' }
  ],
  bodyAttrs: {
    class: 'bg-[#343541]' // Màu nền mặc định để tránh nháy trắng khi load
  }
})
</script>