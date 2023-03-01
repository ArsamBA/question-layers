<template>
   <div class="question-layers">
      <Header />
      <Nuxt />
      <Footer class="footer" />
   </div>
</template>
<script>
   export default {
      components:{
         Header: () => import('@/components/reusable/header.vue'),
         Footer: () => import('@/components/reusable/footer.vue')
      }
   } 
</script>
<style>
</style>