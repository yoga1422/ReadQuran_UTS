<template>
  <NavBar/>
  <router-view />

  <!-- footer -->
  <div class="footer text-light text-center p-3 ">
    Made with Usaha dan Doa by
    <a class="text-light" href="https://github.com/yoga1422">Yoga Pangestu</a>
  </div>
</template>
<script>
import { ref } from "vue";
import axios from "axios";
import NavBar from "../src/components/NavBar.vue";
export default {
  components: {
    NavBar,
  },
};
</script>
<style>
.footer{
  background: #593B30;
}
</style>
