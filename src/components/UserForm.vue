<template>
  <div class="glass-effect card-shadow rounded-3xl p-6 sm:p-10 text-[#1e3a8a] fade-in-up">
    <div class="flex items-center justify-between mb-6">
      <h1 class="text-2xl sm:text-4xl font-bold text-[#1e40af]">
        Учетные записи
      </h1>
      <button
        @click="addData"
        class="button-hover w-12 h-12 bg-white border-2 border-[#3b82f6] text-[#3b82f6] rounded-xl flex items-center justify-center transition-all duration-300"
      >
        <IconPlus class="w-6 h-6" />
      </button>
    </div>

    <div class="flex items-start gap-3 mb-6 p-4 bg-[#dbeafe] rounded-xl border border-[#bfdbfe]">
      <IconSign class="w-5 h-5 text-[#3b82f6] mt-0.5 flex-shrink-0" />
      <p class="text-sm text-[#1e40af] leading-relaxed">
        Для указания нескольких меток для одной пары логин/пароль используйте разделитель ;
      </p>
    </div>

    <div class="bg-white rounded-2xl p-4 sm:p-6 border border-[#bfdbfe] shadow-sm">
      <div class="grid grid-cols-12 gap-4 mb-4 pb-3 border-b border-[#bfdbfe]">
        <div class="col-span-3">
          <div class="text-sm font-semibold text-[#1e3a8a]">Метки</div>
        </div>
        <div class="col-span-3">
          <div class="text-sm font-semibold text-[#1e3a8a]">
            <span class="hidden sm:inline">Тип записи</span>
            <span class="sm:hidden">Тип</span>
          </div>
        </div>
        <div class="col-span-3">
          <div class="text-sm font-semibold text-[#1e3a8a]">Логин</div>
        </div>
        <div class="col-span-3">
          <div class="text-sm font-semibold text-[#1e3a8a]">Пароль</div>
        </div>
      </div>

      <div class="space-y-3">
        <FormRow v-for="(obj, key) in data" :key="obj.id" v-model="data[key]" />
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import IconPlus from './icons/IconPlus.vue'
import IconSign from './icons/IconSign.vue'
import FormRow from './FormRow.vue'
import { useAccountStore } from '@/stores/accountStore'
import { storeToRefs } from 'pinia'

const accountStore = useAccountStore()
const { data } = storeToRefs(accountStore)
const addData = () => accountStore.addData()
</script>

<style scoped>
.glass-effect {
  background: white;
  backdrop-filter: blur(20px);
  border: 1px solid rgba(59, 130, 246, 0.1);
}

.fade-in-up {
  animation: fadeInUp 0.8s ease-out;
}

@keyframes fadeInUp {
  from {
    opacity: 0;
    transform: translateY(40px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.bg-clip-text {
  -webkit-background-clip: text;
  background-clip: text;
}

.button-hover {
  transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
}

.button-hover:hover {
  transform: translateY(-2px);
  box-shadow: 0 20px 25px -5px rgba(37, 99, 235, 0.2), 0 10px 10px -5px rgba(37, 99, 235, 0.1);
}

@media (max-width: 640px) {
  .glass-effect {
    margin: 1rem;
    padding: 1.5rem;
  }
}
</style>
