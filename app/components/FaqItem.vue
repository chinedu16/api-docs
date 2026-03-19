<template>
  <div
    class="border border-white/8 rounded-xl overflow-hidden transition-all duration-200"
    :class="open ? 'bg-white/[0.04]' : 'bg-white/[0.02] hover:bg-white/[0.035]'"
  >
    <!-- Question row -->
    <button
      class="w-full flex items-center justify-between gap-4 px-5 py-4 text-left group"
      @click="open = !open"
    >
      <span
        class="text-sm font-semibold transition-colors duration-150"
        :class="open ? 'text-white' : 'text-slate-300 group-hover:text-white'"
      >
        {{ question }}
      </span>

      <!-- Chevron icon -->
      <span
        class="shrink-0 w-6 h-6 rounded-lg flex items-center justify-center transition-all duration-200"
        :class="open ? 'bg-blue-500/20 text-blue-400 rotate-180' : 'bg-white/5 text-slate-500 group-hover:text-slate-300'"
      >
        <svg class="w-3.5 h-3.5" fill="none" stroke="currentColor" stroke-width="2.5" viewBox="0 0 24 24">
          <path d="m6 9 6 6 6-6"/>
        </svg>
      </span>
    </button>

    <!-- Answer panel -->
    <Transition name="accordion">
      <div v-if="open" class="px-5 pb-5">
        <div class="h-px bg-white/5 mb-4" />
        <p
          class="text-slate-400 text-sm leading-relaxed [&_a]:text-blue-400 [&_a]:underline [&_a]:underline-offset-2 [&_a]:transition [&_a:hover]:text-blue-300"
          v-html="answer"
        />
      </div>
    </Transition>
  </div>
</template>

<script setup lang="ts">
defineProps<{
  question: string
  answer: string
}>()

const open = ref(false)
</script>

<style scoped>
.accordion-enter-active,
.accordion-leave-active {
  transition: opacity 0.2s ease, max-height 0.25s ease;
  max-height: 300px;
  overflow: hidden;
}
.accordion-enter-from,
.accordion-leave-to {
  opacity: 0;
  max-height: 0;
}
</style>