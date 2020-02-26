<template>
  <div class="container">
    <!-- <p>{{$route.params.slug}}</p> -->
    <!-- {{$route.params.post.fields.body}} -->
    {{$route.params.slug}}
  </div>
</template>
<script>
export default {
  data() {
    return {
      blog: null
    };
  },
//   created() {
//     console.log(this.$route.params);
//   }
};
</script>
<style>
</style>