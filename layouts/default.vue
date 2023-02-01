<template>
   <div>
      <Header />
      <Nuxt />
      <Footer />
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