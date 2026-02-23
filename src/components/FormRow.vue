<template>
  <div class="grid grid-cols-12 gap-4 items-center">
    <div class="col-span-3">
      <input
        v-model="tags"
        @blur="handleBlur"
        maxlength="50"
        placeholder="Введите метки"
        class="input-focus w-full h-10 bg-[#eff6ff] border border-[#bfdbfe] rounded-lg px-3 text-[#1e3a8a] placeholder-[#6b93c0]/60 focus:bg-white focus:border-[#3b82f6]"
      />
    </div>

    <div class="col-span-3">
      <select
        @click="hadlerSelect($event)"
        v-model="obj.type"
        class="input-focus w-full h-10 bg-[#eff6ff] border border-[#bfdbfe] rounded-lg px-3 text-[#1e3a8a] cursor-pointer focus:bg-white focus:border-[#3b82f6]"
      >
        <option value="" class="bg-white text-[#1e3a8a]">Выберите опцию</option>
        <option
          @click="setNullPass(obj.type, obj.id)"
          v-for="option in options"
          :key="option.text"
          :value="option.text"
          class="bg-white text-[#1e3a8a]"
        >
          {{ option.text }}
        </option>
      </select>
    </div>

    <div class="col-span-2">
      <input
        v-model="obj.login"
        @blur="handleBlur"
        maxlength="100"
        placeholder="Введите логин"
        class="input-focus w-full h-10 bg-[#eff6ff] border border-[#bfdbfe] rounded-lg px-3 text-[#1e3a8a] placeholder-[#6b93c0]/60 focus:bg-white focus:border-[#3b82f6]"
      />
    </div>

    <div class="col-span-3">
      <input
        v-show="obj.type != 'Local'"
        @blur="handleBlur"
        v-model="obj.pass"
        type="password"
        maxlength="50"
        placeholder="Введите пароль"
        class="input-focus w-full h-10 bg-[#eff6ff] border border-[#bfdbfe] rounded-lg px-3 text-[#1e3a8a] placeholder-[#6b93c0]/60 focus:bg-white focus:border-[#3b82f6]"
      />
    </div>

    <div class="col-span-1 flex justify-end">
      <button
        @click="deleteData(obj.id)"
        class="button-hover bg-[#dbeafe] hover:bg-[#bfdbfe] border border-[#bfdbfe] hover:border-[#3b82f6] text-[#1e3a8a] hover:text-[#1e40af] p-2 rounded-lg transition-all duration-300"
      >
        <IconDelete class="w-4 h-4" />
      </button>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, computed } from 'vue'
import IconDelete from './icons/IconDelete.vue'
import { useAccountStore } from '@/stores/accountStore'
import { storeToRefs } from 'pinia'
const accountStore = useAccountStore()
const deleteData = (id: number) => accountStore.deleteData(id)
const setNullPass = (type: string, id: number) => accountStore.setNullPass(type, id)
const { options } = storeToRefs(accountStore)

const selectElement = ref<HTMLElement | null>(null)
const obj = defineModel({ type: Object })

const tags = computed({
  get() {
    if (Array.isArray(obj.value.tags)) return obj.value.tags.join(';')
    return null
  },
  set(value) {
    obj.value.tags = value.length > 0 ? value.split(';') : []
  },
})

const handleBlur = (event: Event) => {
  const input = event.target as HTMLInputElement
  if (!input.value.trim()) {
    input.classList.add('error-color')
  } else {
    input.classList.remove('error-color')
  }
}

const hadlerSelect = (event: Event) => {
  const target = (event.target as HTMLElement | null)
  if (target && target.nodeName === 'SELECT') {
    selectElement.value = target
    selectElement.value?.classList.remove('error-color')
  } else {
    const selectTarget = event.target as HTMLSelectElement | null
    if (selectTarget && selectTarget.value === '') {
      selectElement.value?.classList.add('error-color')
    } else {
      selectElement.value?.classList.remove('error-color')
    }
  }
}
</script>

<style scoped>
select option {
  background-color: white;
  color: #1e3a8a;
}

input::placeholder {
  color: #6b93c0;
}

.row-enter-active {
  transition: all 0.3s ease;
}

.row-enter-from {
  opacity: 0;
  transform: translateY(-10px);
}

@media (max-width: 610px) {
  .grid {
    display: flex;
    flex-wrap: wrap;
    gap: 0.5rem;
  }

  .col-span-3, .col-span-2, .col-span-1 {
    width: 100%;
  }

  .col-span-1 {
    display: flex;
    justify-content: center;
    margin-top: 0.5rem;
  }
}
</style>
