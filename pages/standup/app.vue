<template>
  <div class="min-h-screen bg-gray-200 text-slate-800">
    <div class="container mx-auto max-w-5xl">
      <Nav />
      <section class="pt-8">
        <div class="mb-4 font-semibold">
          <span class="mr-2 text-3xl">🎉 🥳</span>
          <span class="text-2xl">Recently Completed</span>
        </div>
        <div class="overflow-hidden rounded bg-white p-3">
          <Issues
            v-if="activeSprintId"
            :active-sprint-id="activeSprintId"
            filter="recently-completed"
          />
        </div>
      </section>

      <section class="pt-8">
        <SprintProgress v-if="activeSprintId" :active-sprint-id="activeSprintId" class="mb-8" />
        <div class="overflow-hidden rounded bg-white p-3 shadow">
          <Issues v-if="activeSprintId" :active-sprint-id="activeSprintId" filter="in-progress" />
        </div>
      </section>

      <section class="pt-8">
        <div class="mb-4 font-semibold">
          <span class="mr-2 text-3xl">📝</span>
          <span class="text-2xl">Not Started</span>
        </div>
        <div class="overflow-hidden rounded bg-white p-3 shadow">
          <Issues v-if="activeSprintId" :active-sprint-id="activeSprintId" filter="next-up" />
        </div>
      </section>
      <EndStandupModal v-if="showModal" @close-modal="showModal = false" />
    </div>
  </div>
</template>

<script setup lang="ts">
useHead({
  title: 'Standup',
})

const { data: sprint } = useSprintQuery(2)

const activeSprintId = computed(() => {
  return sprint.value?.values?.[0]?.id
})

const showModal = ref(false)

onMounted(() =>
  window.addEventListener('keydown', e => {
    if (e.key === 'Escape') showModal.value = !showModal.value
  }),
)
</script>
