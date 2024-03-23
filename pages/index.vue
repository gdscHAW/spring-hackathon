<template>
  <div
    v-if="participantList.length === 0"
    class="flex flex-col items-center gap-20 h-full"
  >
    <textarea
      class="w-1/4 h-2/3 rounded-lg text-slate-800 px-5 py-4"
      v-model="participantListText"
    />

    <div class="flex gap-10">
      <input
        class="text-slate-800 px-3 py-2 rounded-md w-20"
        v-model="groupSize"
        type="number"
      />

      <button
        class="bg-white text-slate-800 px-3 py-2 rounded-md"
        @click="handleClick"
      >
        LET'S GO
      </button>
    </div>
  </div>

  <div v-else class="grid grid-cols-3 gap-20 h-full">
    <button class="absolute top-10" @click="handleReset">BACK</button>

    <div
      v-for="(group, index) in participantList"
      class="border border-white rounded-md px-4 py-3 h-fit space-y-4"
      :key="group.join()"
    >
      <h2 class="text-3xl">Group {{ index + 1 }}</h2>
      <p v-html="group.join('<br />')" class="text-2xl"></p>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'

const participantListText = ref<string>('')
const participantList = ref<string[][]>([])
const groupSize = ref<number>(5)

function handleClick(): void {
  const participants: string[] = participantListText.value.split('\n')

  for (let i = participants.length - 1; i > 0; i--) {
    let j = Math.floor(Math.random() * (i + 1))
    let temp = participants[i]
    participants[i] = participants[j]
    participants[j] = temp
  }

  for (let i = 0; i < participants.length; i += groupSize.value) {
    const chunk = participants.slice(i, i + groupSize.value)

    participantList.value.push(chunk)
  }
}

function handleReset(): void {
  participantList.value = []
  participantListText.value = ''
  groupSize.value = 5
}
</script>
