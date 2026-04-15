<script setup>
import { ref, nextTick, onMounted, onUnmounted } from 'vue';
import { Menu, PlusSquare } from 'lucide-vue-next';

// --- Imports des sous-composants ---
// Assure-toi que les chemins correspondent à ton projet
// import ChatSidebar from './ChatSidebar.vue';
// import ChatMessage from './ChatMessage.vue';
// import ChatInput from './ChatInput.vue';
// import WelcomeScreen from './WelcomeScreen.vue';

// --- État de l'application ---
const afficherSidebar = ref(true); 
const conteneurChat = ref(null);
const champSaisieRef = ref(null);
const estEnTrainDeGenerer = ref(false);
const conversations = ref([]);

// --- Gestion du Responsive ---
const gererRedimensionnement = () => {
  if (window.innerWidth < 768) {
    afficherSidebar.value = false;
  } else {
    afficherSidebar.value = true;
  }
};

onMounted(() => {
  gererRedimensionnement();
  window.addEventListener('resize', gererRedimensionnement);
});

onUnmounted(() => {
  window.removeEventListener('resize', gererRedimensionnement);
});

// --- Logique du Chat ---
const gererEnvoiMessage = async (texteMessage) => {
  if (!texteMessage.trim() || estEnTrainDeGenerer.value) return;
  
  // Ajout du message utilisateur
  conversations.value.push({ 
    id: Date.now(), 
    role: 'user', 
    content: texteMessage 
  });
  
  await defilerVersBas();
  estEnTrainDeGenerer.value = true;
  
  // Simulation d'une réponse médicale après 1.2s
  setTimeout(async () => {
    let reponse = "C'est bien noté. Notre équipe spécialisée vous répondra bientôt.";
    const msg = texteMessage.toLowerCase();

    if (msg.includes("rendez-vous") || msg.includes("réserver")) {
      reponse = "Pour prendre rendez-vous, merci de fournir : \n1. Nom complet \n2. Spécialité souhaitée \n3. Créneau préféré.";
    } else if (msg.includes("horaire") || msg.includes("ouvert")) {
      reponse = "L'hôpital est ouvert de **7h00 à 17h00** (Lun-Sam). Les Urgences sont ouvertes **24h/24**.";
    } else if (msg.includes("assurance") || msg.includes("vitale")) {
      reponse = "Nous acceptons la Carte Vitale et la plupart des mutuelles santé.";
    }

    conversations.value.push({
      id: Date.now() + 1,
      role: 'assistant',
      content: reponse
    });

    estEnTrainDeGenerer.value = false;
    await defilerVersBas();
  }, 1200);
};

const defilerVersBas = async () => {
  await nextTick();
  if (conteneurChat.value) {
    conteneurChat.value.scrollTop = conteneurChat.value.scrollHeight;
  }
};

const demarrerNouveauChat = () => {
  conversations.value = [];
  if (window.innerWidth < 768) afficherSidebar.value = false;
  nextTick(() => champSaisieRef.value?.focus());
};
</script>

<template>
  <div class="flex h-screen bg-white text-slate-800 font-sans overflow-hidden">
    
    <ChatSidebar 
      :is-open="afficherSidebar" 
      @close="afficherSidebar = false" 
      @new-chat="demarrerNouveauChat" 
    />

    <main class="flex-1 flex flex-col relative h-full bg-white">
      
      <div class="absolute top-0 left-0 right-0 p-3 z-10 flex items-center justify-between pointer-events-none">
        <div class="pointer-events-auto">
          <button 
            v-if="!afficherSidebar"
            @click="afficherSidebar = true"
            class="hidden md:flex p-2 text-slate-500 hover:bg-blue-50 hover:text-blue-600 rounded-md transition-colors"
          >
            <Menu :size="24" />
          </button>
        </div>
        <div class="pointer-events-auto">
          <button 
            @click="demarrerNouveauChat" 
            class="hidden md:flex p-2 text-slate-500 hover:bg-blue-50 hover:text-blue-600 rounded-md transition-colors"
            title="Nouvelle consultation"
          >
            <PlusSquare :size="20" />
          </button>
        </div>
      </div>

      <header class="flex items-center justify-between p-3 md:hidden bg-white border-b border-slate-100 z-10 shadow-sm">
        <button @click="afficherSidebar = !afficherSidebar" class="p-2 text-slate-600 hover:bg-blue-50 rounded-md">
          <Menu :size="24" />
        </button>
        <span class="font-bold text-blue-700">Hôpital Tâm Đức</span>
        <button @click="demarrerNouveauChat" class="p-2 text-slate-600 hover:bg-blue-50 rounded-md">
          <PlusSquare :size="20" />
        </button>
      </header>

      <div ref="conteneurChat" class="flex-1 overflow-y-auto w-full scroll-smooth">
        <div class="flex flex-col min-h-full">
          
          <WelcomeScreen 
            v-if="conversations.length === 0" 
            @suggestion-click="gererEnvoiMessage" 
          />
          
          <div v-else class="flex flex-col pb-36 md:pb-40 pt-16 md:pt-14">
            <ChatMessage 
              v-for="(msg, index) in conversations" 
              :key="msg.id"
              :message="msg"
              :is-generating="estEnTrainDeGenerer"
              :is-last="index === conversations.length - 1"
            />
          </div>
          
        </div>
      </div>

      <ChatInput 
        ref="champSaisieRef" 
        :loading="estEnTrainDeGenerer" 
        @send="gererEnvoiMessage" 
      />

    </main>
  </div>
</template>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap');

:deep(body) {
  font-family: 'Inter', sans-serif;
}

/* Scrollbar invisible pour Chrome/Safari */
::-webkit-scrollbar {
  width: 0px;
  background: transparent;
}
</style>