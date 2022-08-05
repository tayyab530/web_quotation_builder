<script>
export default {
  data(){
    return {
      newTitle:'Title will be here'
    }
  },
  emits: ['editTitle'],
  created() {
    this.$emit('editTitle', newTitle)
  }
}
</script>

<template>
  <div class="p-10">
    <div>
      <label>Company Title </label><input v-model="newTitle" placeholder="Enter company title" />
    </div>
    <br />
    <div>
      <label>Company Logo URL </label
      ><input placeholder="Enter company Logo URL" class="bg-red-500" />
    </div>
  </div>
</template>
