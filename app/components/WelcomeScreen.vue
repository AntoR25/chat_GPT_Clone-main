<script setup>
import { Stethoscope, CalendarCheck, Clock, ShieldCheck } from 'lucide-vue-next';
defineEmits(['suggestion-click']);
</script>

<template>
  <div class="flex-1 flex flex-col items-center justify-center text-center px-4 pb-32 animate-fade-in">
    <div class="bg-blue-50 p-6 rounded-full mb-6 border-4 border-blue-100 shadow-sm">
      <Stethoscope :size="64" class="text-blue-600" />
    </div>
    
    <h2 class="text-3xl font-bold mb-2 text-slate-800">Bonjour, comment puis-je vous aider ?</h2>
    <p class="text-sm text-slate-500 mb-10 max-w-md mx-auto">
      Le service client de l'Hôpital Tâm Đức est disponible 24h/24 et 7j/7. 
      <br>Veuillez choisir un thème ou saisir votre question ci-dessous.
    </p>
    
    <div class="grid grid-cols-1 md:grid-cols-2 gap-4 max-w-3xl w-full">
      <button 
        @click="$emit('suggestion-click', 'Je souhaite prendre rendez-vous en médecine interne.')" 
        class="flex items-center gap-4 bg-white hover:bg-blue-50 p-4 rounded-xl text-sm text-slate-600 text-left transition-all border border-slate-200 hover:border-blue-300 shadow-sm hover:shadow-md group"
      >
        <div class="bg-blue-100 p-2 rounded-lg text-blue-600 group-hover:bg-blue-600 group-hover:text-white transition-colors">
            <CalendarCheck :size="20"/>
        </div>
        <span class="font-medium">"Prendre rendez-vous"</span>
      </button>

      <button 
        @click="$emit('suggestion-click', 'Quels sont les horaires d\'ouverture de l\'hôpital ?')" 
        class="flex items-center gap-4 bg-white hover:bg-blue-50 p-4 rounded-xl text-sm text-slate-600 text-left transition-all border border-slate-200 hover:border-blue-300 shadow-sm hover:shadow-md group"
      >
        <div class="bg-green-100 p-2 rounded-lg text-green-600 group-hover:bg-green-600 group-hover:text-white transition-colors">
            <Clock :size="20"/>
        </div>
        <span class="font-medium">"Horaires d'ouverture"</span>
      </button>

      <button 
        @click="$emit('suggestion-click', 'Quels sont les symptômes de la grippe saisonnière ?')" 
        class="flex items-center gap-4 bg-white hover:bg-blue-50 p-4 rounded-xl text-sm text-slate-600 text-left transition-all border border-slate-200 hover:border-blue-300 shadow-sm hover:shadow-md group"
      >
        <div class="bg-orange-100 p-2 rounded-lg text-orange-600 group-hover:bg-orange-600 group-hover:text-white transition-colors">
            <Stethoscope :size="20"/>
        </div>
        <span class="font-medium">"Conseils sur des symptômes"</span>
      </button>

      <button 
        @click="$emit('suggestion-click', 'Comment fonctionne la prise en charge de l\'assurance maladie ?')" 
        class="flex items-center gap-4 bg-white hover:bg-blue-50 p-4 rounded-xl text-sm text-slate-600 text-left transition-all border border-slate-200 hover:border-blue-300 shadow-sm hover:shadow-md group"
      >
        <div class="bg-purple-100 p-2 rounded-lg text-purple-600 group-hover:bg-purple-600 group-hover:text-white transition-colors">
            <ShieldCheck :size="20"/>
        </div>
        <span class="font-medium">"Assurance et prise en charge"</span>
      </button>
    </div>
  </div>
</template>