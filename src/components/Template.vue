<script>
// let width = 30;

export default {
  props: {
    title: String,
    logoUrl: String,
  },

  data() {
    return {
      // bg-width: width,
      // bg-height: width * 1.4142,
    };
  },
  // 1:1.4142 A4 ratio
};
</script>

<template>
  <div
    class="
      bg-[url('https://png.pngtree.com/thumb_back/fw800/background/20200408/pngtree-a4-layered-paper-contrast-color-background-image_333554.jpg')]
      bg-no-repeat bg-contain
      flex flex-col
      items-start
      h-[1000px]
      w-[685px]
      pl-6
      pt-6
      space-y-2
    "
  >
    <div class="flex flex-col w-full">
      <img class="h-24 w-24 hover:shadow-md" :src="logoUrl" alt="Your logo will be here" />
      <div class="flex flex-col items-center">
        <h2 class="flex font-bold text-5xl text-blue-200 mt-[100px] text-center hover:shadow-md">
          {{ title || 'Your Cover Title' }}
        </h2>
      </div>
    </div>
  </div>
</template>
