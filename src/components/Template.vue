<script>
// let width = 30;

export default {
  props: {
    title: String,
    logoUrl: String,
  },

  data(){
    return {
      // bg-width: width,
      // bg-height: width * 1.4142,
    }
  }
  // 1:1.4142 A4 ratio
};
</script>

<template>
  <div
    class="
      bg-[url('https://png.pngtree.com/thumb_back/fw800/background/20200408/pngtree-a4-layered-paper-contrast-color-background-image_333554.jpg')]
      h-110
      w-85
      bg-no-repeat
      bg-contain
    "
  >
    <div class="h-screen w-screen p-10">
      <img class="h-24 w-24" :src="logoUrl" alt="Your logo will be here" />
      <h2 class="font-bold text-5xl text-black" >{{ title || 'Title of the cover' }}</h2>
    </div>
  </div>
</template>
