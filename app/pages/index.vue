<script setup>
import { ref, nextTick, onMounted, onUnmounted } from 'vue';
import { Menu, PlusSquare } from 'lucide-vue-next';

// --- State ---
const showSidebar = ref(true); 
const chatContainer = ref(null);
const chatInputRef = ref(null);
const isGenerating = ref(false);

const handleResize = () => {
  if (window.innerWidth < 768) showSidebar.value = false;
  else showSidebar.value = true;
};

onMounted(() => {
  handleResize();
  window.addEventListener('resize', handleResize);
});
onUnmounted(() => window.removeEventListener('resize', handleResize));

// --- Chat Logic ---
const conversations = ref([]);

const handleSendMessage = async (messageText) => {
  if (isGenerating.value) return;
  conversations.value.push({ id: Date.now(), role: 'user', content: messageText });
  await scrollToBottom();
  isGenerating.value = true;
  
  // Logic phản hồi giả lập Y tế
  setTimeout(async () => {
    let response = "Dạ, em đã ghi nhận câu hỏi. Bộ phận chuyên môn sẽ phản hồi ngay ạ.";
    const lowerMsg = messageText.toLowerCase();

    if (lowerMsg.includes("đặt lịch") || lowerMsg.includes("khám")) {
      response = "Để đặt lịch khám, bạn vui lòng cung cấp: \n1. Họ tên bệnh nhân \n2. Số điện thoại \n3. Chuyên khoa muốn khám (hoặc triệu chứng) \n4. Thời gian dự kiến.";
    } else if (lowerMsg.includes("giờ") || lowerMsg.includes("thời gian")) {
      response = "Bệnh viện làm việc từ **7:00 - 17:00** (Thứ 2 - Thứ 7). \nKhoa Cấp cứu hoạt động **24/24** tất cả các ngày trong tuần.";
    } else if (lowerMsg.includes("bảo hiểm") || lowerMsg.includes("bhyt")) {
      response = "Bệnh viện có tiếp nhận BHYT đúng tuyến và trái tuyến (đối với trường hợp nội trú). Vui lòng mang theo thẻ BHYT và CCCD khi đến đăng ký.";
    } else if (lowerMsg.includes("triệu chứng") || lowerMsg.includes("đau")) {
      response = "Dựa trên mô tả, bạn nên đến khám tại chuyên khoa **Nội Tổng Quát** để được bác sĩ chẩn đoán chính xác. \n*Lưu ý: Nếu cơn đau dữ dội, vui lòng đến ngay phòng Cấp cứu.*";
    }

    conversations.value.push({
      id: Date.now() + 1,
      role: 'assistant',
      content: response
    });
    isGenerating.value = false;
    await scrollToBottom();
  }, 1200);
};

const scrollToBottom = async () => {
  await nextTick();
  if (chatContainer.value) chatContainer.value.scrollTop = chatContainer.value.scrollHeight;
};

const startNewChat = () => {
  conversations.value = [];
  if (window.innerWidth < 768) showSidebar.value = false;
  nextTick(() => chatInputRef.value?.focus());
};
</script>

<template>
  <div class="flex h-screen bg-white text-slate-800 font-sans overflow-hidden">
    
    <ChatSidebar :is-open="showSidebar" @close="showSidebar = false" @new-chat="startNewChat" />

    <main class="flex-1 flex flex-col relative h-full bg-white">
      
      <!-- TOP BAR -->
      <div class="absolute top-0 left-0 right-0 p-3 z-10 flex items-center justify-between pointer-events-none">
        <div class="pointer-events-auto">
          <button 
            v-if="!showSidebar"
            @click="showSidebar = true"
            class="hidden md:flex p-2 text-slate-500 hover:bg-blue-50 hover:text-blue-600 rounded-md transition-colors"
          >
            <Menu :size="24" />
          </button>
        </div>
        <div class="pointer-events-auto">
          <button 
            @click="startNewChat" 
            class="hidden md:flex p-2 text-slate-500 hover:bg-blue-50 hover:text-blue-600 rounded-md transition-colors"
            title="Cuộc tư vấn mới"
          >
             <PlusSquare :size="20"/>
          </button>
        </div>
      </div>

      <!-- MOBILE HEADER -->
      <header class="flex items-center justify-between p-3 md:hidden bg-white border-b border-slate-100 z-10 shadow-sm">
        <button @click="showSidebar = !showSidebar" class="p-2 text-slate-600 hover:bg-blue-50 rounded-md">
          <Menu :size="24" />
        </button>
        <span class="font-bold text-blue-700">BV Tâm Đức</span>
        <button @click="startNewChat" class="p-2 text-slate-600 hover:bg-blue-50 rounded-md">
          <PlusSquare :size="20" />
        </button>
      </header>

      <!-- Chat Container -->
      <div ref="chatContainer" class="flex-1 overflow-y-auto w-full scroll-smooth">
        <div class="flex flex-col min-h-full">
          <WelcomeScreen v-if="conversations.length === 0" @suggestion-click="handleSendMessage" />
          <div v-else class="flex flex-col pb-36 md:pb-40 pt-16 md:pt-14">
            <ChatMessage 
              v-for="(msg, index) in conversations" 
              :key="msg.id"
              :message="msg"
              :is-generating="isGenerating"
              :is-last="index === conversations.length - 1"
            />
          </div>
        </div>
      </div>

      <ChatInput ref="chatInputRef" :loading="isGenerating" @send="handleSendMessage" />

    </main>
  </div>
</template>