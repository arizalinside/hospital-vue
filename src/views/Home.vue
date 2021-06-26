<template>
  <div class="home">
    <Create msg="Create new patient" />
  </div>
</template>

<script>
// @ is an alias to /src
import Create from '@/components/CreatePatient.vue';

export default {
  name: 'Home',
  components: {
    Create,
  },
};
</script>
