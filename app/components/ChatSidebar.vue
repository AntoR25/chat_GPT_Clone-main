<script setup>
import { 
  Plus, MessageSquare, Settings, Phone, Calendar, 
  FileText, Activity, Menu, CreditCard, Stethoscope
} from 'lucide-vue-next';

defineProps({
  isOpen: { type: Boolean, default: true }
});

const emit = defineEmits(['close', 'new-chat']);
</script>

<template>
  <div>
    <div 
      v-if="isOpen" 
      class="fixed inset-0 bg-blue-900/20 z-20 md:hidden transition-opacity backdrop-blur-[2px]"
      @click="emit('close')"
    ></div>

    <aside 
      :class="[
        'fixed inset-y-0 left-0 z-30 flex flex-col h-full bg-[#f0f9ff] text-slate-700 transition-all duration-300 ease-in-out border-r border-blue-100',
        isOpen ? 'translate-x-0 w-[280px]' : '-translate-x-full w-[280px]',
        'md:relative md:translate-x-0',
        isOpen ? 'md:w-[280px]' : 'md:w-0 md:overflow-hidden'
      ]"
    >
      <div class="px-4 pt-4 pb-2 min-w-[280px]">
        <div class="flex items-center justify-between mb-6">
          <div class="flex items-center gap-2 text-blue-700 font-bold text-lg">
            <div class="bg-blue-600 p-1 rounded-md text-white">
              <Activity :size="20" />
            </div>
            <span>Hôpital Tâm Đức</span>
          </div>
          <button @click="emit('close')" class="md:hidden text-slate-400 hover:text-blue-600">
            <Menu :size="24" />
          </button>
        </div>

        <button 
          @click="emit('new-chat')"
          class="flex items-center justify-center gap-2 w-full bg-blue-600 hover:bg-blue-700 text-white px-4 py-3 rounded-xl transition-all shadow-md shadow-blue-200 font-medium"
        >
          <Plus :size="18" />
          <span>Nouvelle consultation</span>
        </button>
      </div>

      <div class="flex-1 overflow-y-auto px-3 py-2 custom-scrollbar min-w-[280px]">
        
        <div class="mb-6">
          <h3 class="px-3 text-xs font-bold text-blue-400 mb-2 uppercase tracking-wider">Services rapides</h3>
          <button class="flex items-center gap-3 w-full px-3 py-2.5 text-sm text-slate-600 hover:bg-white hover:text-blue-600 hover:shadow-sm rounded-lg transition-all text-left">
            <Calendar :size="18" class="text-blue-500" />
            <span>Prendre rendez-vous</span>
          </button>
          <button class="flex items-center gap-3 w-full px-3 py-2.5 text-sm text-slate-600 hover:bg-white hover:text-blue-600 hover:shadow-sm rounded-lg transition-all text-left">
            <CreditCard :size="18" class="text-green-500" />
            <span>Tarifs des services</span>
          </button>
           <button class="flex items-center gap-3 w-full px-3 py-2.5 text-sm text-slate-600 hover:bg-white hover:text-blue-600 hover:shadow-sm rounded-lg transition-all text-left">
            <FileText :size="18" class="text-orange-500" />
            <span>Assurance Maladie</span>
          </button>
        </div>

        <div>
          <h3 class="px-3 text-xs font-bold text-blue-400 mb-2 uppercase tracking-wider">Historique</h3>
          
          <button class="flex items-center gap-3 w-full px-3 py-2.5 text-sm bg-white text-blue-700 font-medium rounded-lg mb-2 shadow-sm border border-blue-100 text-left">
            <Stethoscope :size="16" />
            <span class="truncate">Symptômes de la dengue</span>
          </button>

          <button class="flex items-center gap-3 w-full px-3 py-2.5 text-sm text-slate-500 hover:bg-white hover:text-blue-600 rounded-lg transition-all text-left group">
            <MessageSquare :size="16" class="shrink-0 group-hover:text-blue-400" />
            <span class="truncate">Horaires de la pédiatrie...</span>
          </button>
          
          <button class="flex items-center gap-3 w-full px-3 py-2.5 text-sm text-slate-500 hover:bg-white hover:text-blue-600 rounded-lg transition-all text-left group">
             <MessageSquare :size="16" class="shrink-0 group-hover:text-blue-400" />
            <span class="truncate">Procédure d'admission...</span>
          </button>
        </div>
      </div>

      <div class="p-4 border-t border-blue-100 min-w-[280px] bg-[#f0f9ff]">
        <div class="bg-red-50 border border-red-100 rounded-lg p-3 flex items-start gap-3 mb-3">
            <Phone :size="18" class="text-red-500 mt-0.5 shrink-0" />
            <div>
                <p class="text-xs font-bold text-red-600">Urgence : 15</p>
                <p class="text-[10px] text-red-400">Hotline : 1900 1234</p>
            </div>
        </div>
        
        <button class="flex items-center gap-3 w-full px-2 py-2 text-sm text-slate-600 hover:text-blue-600 transition-colors">
          <Settings :size="18" />
          <span>Paramètres du compte</span>
        </button>
      </div>
    </aside>
  </div>
</template>

<style scoped>
.custom-scrollbar::-webkit-scrollbar { width: 5px; }
.custom-scrollbar::-webkit-scrollbar-thumb { background: #cbd5e1; border-radius: 10px; }
.custom-scrollbar::-webkit-scrollbar-thumb:hover { background: #94a3b8; }
</style>