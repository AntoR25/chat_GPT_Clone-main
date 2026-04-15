<script setup>
import { User, Stethoscope } from 'lucide-vue-next';

const props = defineProps({
  message: { type: Object, required: true },
  isGenerating: Boolean,
  isLast: Boolean
});

const isAssistant = props.message.role === 'assistant';
</script>

<template>
  <div 
    :class="[
      'w-full py-8 border-b border-slate-100',
      isAssistant ? 'bg-[#f8fafc]' : 'bg-white'
    ]"
  >
    <div class="max-w-3xl mx-auto flex gap-5 px-4 md:px-0 text-slate-700 text-[16px] leading-relaxed">
      <!-- Avatar -->
      <div class="flex-shrink-0 flex flex-col relative items-end">
        <div 
          class="w-10 h-10 rounded-full flex items-center justify-center border shadow-sm"
          :class="isAssistant ? 'bg-blue-600 border-blue-600' : 'bg-white border-slate-200'"
        >
          <Stethoscope v-if="isAssistant" :size="20" class="text-white" />
          <User v-else :size="20" class="text-slate-500" />
        </div>
        <!-- Name Tag -->
        <span class="text-[10px] font-bold mt-1 text-slate-400 uppercase tracking-wide">
            {{ isAssistant ? 'Hospital' : 'Bạn' }}
        </span>
      </div>
      
      <!-- Content -->
      <div class="relative flex-1 overflow-hidden pt-1">
        <div class="prose prose-slate max-w-none whitespace-pre-wrap break-words">
          {{ message.content }}
          <span v-if="isGenerating && isLast && isAssistant" class="inline-block w-2 h-5 ml-1 align-middle bg-blue-600 animate-pulse rounded-full"></span>
        </div>
      </div>
    </div>
  </div>
</template>