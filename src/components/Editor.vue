<script>
import Style from '../styles/field.module.css'
export default {
  data() {
    return {
      newTitle: '',
      logoUrl: '',
    };
  },

  methods: {
    changeTitle: function () {
      this.$emit('editTitle', this.newTitle);
    },

    onInput(e) {
      this.newTitle = e.target.value;
      this.$emit('editTitle', this.newTitle);
    },

    onInputUrl(e){
      this.logoUrl = e.target.value;
      this.$emit('editUrl', this.logoUrl);
    }
  },
};
</script>

<template>
  <div class="flex flex-col space-y-8 items-start mr-5 mt-6">
    <div class={Style.field}>
      <label>Company Title </label
      ><input
        v-bind="newTitle"
        @input="onInput"
        placeholder="Enter company title"
      />
    </div>
    <div class={Style.field}>
      <label>Company Logo URL </label
      ><input placeholder="Enter company Logo URL" />
    </div>
  </div>
</template>