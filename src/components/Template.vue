<script>
export default {
  props: {
    title: String
  }
}
</script>

<template>
  <div class="bg-[url('https://templatelab.com/wp-content/uploads/2019/03/Cover-Page-Template-1-TemplateLab-e1553052868262.jpg')] h-300 w-200">
    <div class="h-300 w-200"> 
      <img
        src="https://www.nicepng.com/png/detail/246-2467547_your-logo-here-your-logo-here-logo-png.png"
        alt="Your logo will be here"
      />
      <h2 style="color: black">{{title || "Title of the cover"}}</h2>
    </div>
  </div>
</template>
