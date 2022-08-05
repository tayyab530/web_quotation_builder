<script >
// This starter template is using Vue 3 <script setup> SFCs
// Check out https://vuejs.org/api/sfc-script-setup.html#script-setup
import Editor from './components/Editor.vue';
import Template from './components/Template.vue';

export default {
  components: {
    Template
  },
  data() {
    return {
      coverTitle: 'Hello from parent',
    }
  },
}
</script>

<template>
  <div>
    <Editor @editTitle="(newTitle) => coverTitle = newTitle"></Editor>
    <Template :title="coverTitle"></Template>
  </div>
</template>
