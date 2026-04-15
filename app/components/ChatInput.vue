<script setup>
import { ref, onMounted } from 'vue';
import { Send, AlertCircle } from 'lucide-vue-next';

const props = defineProps({ chargement: Boolean });
const emit = defineEmits(['envoyer']);

const champMessage = ref(null);
const messageUtilisateur = ref('');

// Gère l'envoi du message
const envoyerMessage = () => {
  if (!messageUtilisateur.value.trim() || props.chargement) return;
  
  emit('envoyer', messageUtilisateur.value);
  messageUtilisateur.value = '';
  
  // Réinitialise la hauteur après l'envoi
  if (champMessage.value) champMessage.value.style.height = '50px';
};

// Ajuste dynamiquement la hauteur du textarea selon le contenu
const ajusterHauteur = (e) => {
  e.target.style.height = 'auto';
  e.target.style.height = `${e.target.scrollHeight}px`;
};

onMounted(() => champMessage.value?.focus());
defineExpose({ focus: () => champMessage.value?.focus() });
</script>

<template>
  <div class="absolute bottom-0 left-0 w-full bg-gradient-to-t from-white via-white to-transparent pt-10 pb-4 px-4">
    <div class="max-w-3xl mx-auto w-full">
      
      <div class="relative flex items-end w-full p-2 bg-white rounded-2xl shadow-[0_4px_20px_rgba(0,0,0,0.08)] border border-slate-200 focus-within:ring-2 focus-within:ring-blue-100 focus-within:border-blue-400 overflow-hidden transition-all">
        <textarea 
          ref="champMessage"
          v-model="messageUtilisateur"
          @keydown.enter.exact.prevent="envoyerMessage"
          @input="ajusterHauteur"
          placeholder="Posez votre question au conseiller..." 
          class="w-full bg-transparent text-slate-700 placeholder-slate-400 focus:outline-none resize-none m-0 max-h-[200px] py-3 pl-3 pr-12 text-base custom-scrollbar"
          style="min-height: 24px; height: 24px;"
          rows="1"
        ></textarea>
        
        <button 
          @click="envoyerMessage"
          :disabled="!messageUtilisateur.trim() || chargement"
          class="absolute right-2 bottom-2 p-2 rounded-xl text-white bg-blue-600 hover:bg-blue-700 disabled:bg-slate-200 disabled:text-slate-400 disabled:cursor-not-allowed transition-all duration-200 shadow-sm"
        >
          <Send :size="18" />
        </button>
      </div>

      <div class="text-center pt-3 pb-1 flex items-center justify-center gap-2 opacity-80">
        <AlertCircle :size="12" class="text-red-500" />
        <span class="text-[11px] text-slate-500">
          L'IA fournit uniquement des conseils informatifs. En cas d'<strong>urgence</strong>, contactez immédiatement le <strong>15</strong> (ou le 112).
        </span>
      </div>
    </div>
  </div>
</template>

<style scoped>
/* Masquer la barre de défilement pour un look plus propre */
textarea { scrollbar-width: none; -ms-overflow-style: none; }
textarea::-webkit-scrollbar { display: none; }
</style>