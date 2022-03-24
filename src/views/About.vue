<template>
  <div class="about">
    <HeaderShayna />
    <FooterShayna />
  </div>
</template>

<script>
import HeaderShayna from '@/components/HeaderShayna.vue';
import FooterShayna from '@/components/FooterShayna.vue';

export default {
  name: 'Product',
  components: {
    HeaderShayna,
    FooterShayna,
  },
};
</script>
