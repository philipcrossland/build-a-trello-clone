<template>
  <AppDrop @drop="moveTaskOrColumn">
    <AppDrag
      class="task"
      :transferData="{
        type: 'task',
        fromColumnIndex: columnIndex,
        fromTaskIndex: taskIndex
      }"
      @click="goToTask(task.id)"
    >
      <span class="w-full flex-no-shrink font-bold">{{ task.name }}</span>
      <p v-if="task.description" class="w-full flex-no-shrink mt-1 text-sm">
        {{ task.description }}
      </p>
    </AppDrag>
  </AppDrop>
</template>

<script>
import AppDrag from '@/components/AppDrag'
import AppDrop from '@/components/AppDrop'
import movingTasksAndColumnsMixins from '@/mixins/movingTasksAndColumnsMixin'

export default {
  components: {
    AppDrag,
    AppDrop
  },
  props: {
    task: {
      type: Object,
      required: true
    },
    taskIndex: {
      type: Number,
      required: true
    }
  },
  mixins: [movingTasksAndColumnsMixins],
  methods: {
    goToTask (taskId) {
      this.$router.push({ name: 'task', params: { id: taskId } })
    }
  }
}
</script>

<style lang="css" scoped>
.task {
  @apply flex items-center flex-wrap shadow mb-2 py-2 px-2 rounded bg-white text-grey-darkest no-underline;
}
</style>
