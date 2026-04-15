<script setup>
import { Stethoscope, CalendarCheck, Clock, ShieldCheck } from 'lucide-vue-next';
defineEmits(['suggestion-click']);
</script>

<template>
  <div class="flex-1 flex flex-col items-center justify-center text-center px-4 pb-32 animate-fade-in">
    <div class="bg-blue-50 p-6 rounded-full mb-6 border-4 border-blue-100 shadow-sm">
      <Stethoscope :size="64" class="text-blue-600" />
    </div>
    
    <h2 class="text-3xl font-bold mb-2 text-slate-800">Xin chào, tôi có thể giúp gì?</h2>
    <p class="text-sm text-slate-500 mb-10 max-w-md mx-auto">
      Hệ thống CSKH Bệnh viện Tâm Đức hỗ trợ tư vấn 24/7. 
      <br>Vui lòng chọn chủ đề hoặc nhập câu hỏi bên dưới.
    </p>
    
    <div class="grid grid-cols-1 md:grid-cols-2 gap-4 max-w-3xl w-full">
      <button 
        @click="$emit('suggestion-click', 'Tôi muốn đặt lịch khám chuyên khoa Nội?')" 
        class="flex items-center gap-4 bg-white hover:bg-blue-50 p-4 rounded-xl text-sm text-slate-600 text-left transition-all border border-slate-200 hover:border-blue-300 shadow-sm hover:shadow-md group"
      >
        <div class="bg-blue-100 p-2 rounded-lg text-blue-600 group-hover:bg-blue-600 group-hover:text-white transition-colors">
            <CalendarCheck :size="20"/>
        </div>
        <span class="font-medium">"Tôi muốn đặt lịch khám?"</span>
      </button>

      <button 
        @click="$emit('suggestion-click', 'Bệnh viện làm việc khung giờ nào?')" 
        class="flex items-center gap-4 bg-white hover:bg-blue-50 p-4 rounded-xl text-sm text-slate-600 text-left transition-all border border-slate-200 hover:border-blue-300 shadow-sm hover:shadow-md group"
      >
        <div class="bg-green-100 p-2 rounded-lg text-green-600 group-hover:bg-green-600 group-hover:text-white transition-colors">
            <Clock :size="20"/>
        </div>
        <span class="font-medium">"Giờ làm việc của bệnh viện?"</span>
      </button>

      <button 
        @click="$emit('suggestion-click', 'Các triệu chứng của bệnh cúm mùa?')" 
        class="flex items-center gap-4 bg-white hover:bg-blue-50 p-4 rounded-xl text-sm text-slate-600 text-left transition-all border border-slate-200 hover:border-blue-300 shadow-sm hover:shadow-md group"
      >
        <div class="bg-orange-100 p-2 rounded-lg text-orange-600 group-hover:bg-orange-600 group-hover:text-white transition-colors">
            <Stethoscope :size="20"/>
        </div>
        <span class="font-medium">"Tư vấn triệu chứng bệnh?"</span>
      </button>

      <button 
        @click="$emit('suggestion-click', 'Bệnh viện có chấp nhận BHYT trái tuyến không?')" 
        class="flex items-center gap-4 bg-white hover:bg-blue-50 p-4 rounded-xl text-sm text-slate-600 text-left transition-all border border-slate-200 hover:border-blue-300 shadow-sm hover:shadow-md group"
      >
        <div class="bg-purple-100 p-2 rounded-lg text-purple-600 group-hover:bg-purple-600 group-hover:text-white transition-colors">
            <ShieldCheck :size="20"/>
        </div>
        <span class="font-medium">"Quy định Bảo hiểm Y tế?"</span>
      </button>
    </div>
  </div>
</template>