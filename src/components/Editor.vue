<script>
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
  <div class="p-10">
    <div>
      <label>Company Title </label
      ><input
        v-bind="newTitle"
        @input="onInput"
        placeholder="Enter company title"
      />
    </div>
    <br />
    <div>
      <label>Company Logo URL </label
      ><input placeholder="Enter company Logo URL" class="bg-red-500" />
    </div>
  </div>
</template>
