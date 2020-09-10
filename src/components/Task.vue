<template>
  <div :key="task.id" class="mainWrap">
    <input
      type="checkbox"
      :id="'todo-'+index"
      v-model="task.is_finished"
      @change="() => updateStatus(task.id, task.is_finished)"
      :class="{'checked':task.is_finished}"
    />
    <label
      :class="{'finish':task.is_finished}"
      :for="'todo-'+index"
      @dblclick="retext()"
    >{{task.name}}</label>
    <button @click="() => deleteTask(task.id)" class="mainDelate">
      <font-awesome-icon :icon="['far', 'trash-alt']" class="garbege" />
    </button>
  </div>
</template>

<script>
export default {
  props: {
    task,
  },
};
</script>