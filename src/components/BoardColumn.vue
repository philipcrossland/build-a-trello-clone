<template>
  <AppDrop @drop="moveTaskOrColumn">
    <AppDrag
      :transferData="{
        type: 'column',
        fromColumnIndex: columnIndex
      }"
      class="column"
    >
      <div class="flex items-center mb-2 font-bold">{{ column.name }}</div>
      <div class="list-reset">
        <ColumnTask
          v-for="(task, taskIndex) of column.tasks"
          :key="taskIndex"
          :task="task"
          :taskIndex="taskIndex"
          :column="column"
          :columnIndex="columnIndex"
          :board="board"
        />

        <input
          type="text"
          class="block p-2 w-full bg-transparent"
          placeholder="+ Enter new task"
          @keyup.enter="createTask($event, column.tasks)"
        />
      </div>
    </AppDrag>
  </AppDrop>
</template>

<script>
import ColumnTask from '@/components/ColumnTask'
import AppDrag from '@/components/AppDrag'
import AppDrop from '@/components/AppDrop'
import movingTasksAndColumnsMixins from '@/mixins/movingTasksAndColumnsMixin'

export default {
  components: {
    ColumnTask,
    AppDrag,
    AppDrop
  },
  mixins: [movingTasksAndColumnsMixins],
  methods: {
    pickupColumn (e, fromColumnIndex) {
      e.dataTransfer.effectAllowed = 'move'
      e.dataTransfer.dropEffect = 'move'

      e.dataTransfer.setData('from-column-index', fromColumnIndex)
      e.dataTransfer.setData('type', 'column')
    },
    createTask (e, tasks) {
      this.$store.commit('CREATE_TASK', {
        tasks,
        name: e.target.value
      })

      e.target.value = ''
    }
  }
}
</script>

<style lang="css" scoped>
.column {
  @apply bg-grey-light p-2 mr-4 text-left shadow rounded;
  min-width: 350px;
}
</style>
