<script>
// This starter template is using Vue 3 <script setup> SFCs
// Check out https://vuejs.org/api/sfc-script-setup.html#script-setup
import Editor from './components/Editor.vue';
import Template from './components/Template.vue';

export default {
  components: {
    Template,
    Editor,
  },
  data() {
    return {
      coverTitle: 'Your Cover Title',
      logoURL:
        'https://www.pngkey.com/png/full/360-3601772_your-logo-here-your-company-logo-here-png.png',
    };
  },
  methods: {
    updateTitle(newTitle) {
      this.coverTitle = newTitle;
      // alert("hello child" + newTitle)
    },
  },
};
</script>

<template>
  <div class="flex flex-row ">
    <Editor v-on:editTitle="updateTitle"></Editor>
    <Template :title="coverTitle" :logoUrl="logoURL"></Template>
  </div>
</template>
