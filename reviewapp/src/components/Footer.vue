<template>
  <footer class="footer">
    <!-- License info -->
    <div class="content has-text-centered">
      <p>
        <strong>Built</strong> by
        <a href="https://github.com/alexisdavalos" target="_blank"
          >Alexis Davalos</a
        >. The source code is licensed under open source
        <a href="http://opensource.org/licenses/mit-license.php" target="_blank"
          >MIT</a
        >
        License.
      </p>
    </div>

    <!-- Alexis Davalos Logo -->
    <div class="logos columns is-mobile is-centered">
      <figure class="image is-128x128">
        <a href="https://www.alexisdavalos.dev" target="_blank"
          ><img :src="image" alt="alexis davalos logo"
        /></a>
      </figure>
    </div>
  </footer>
</template>
<script>
import alexis_davalos_logo from "../assets/alexis_davalos_logo.png";
export default {
  data() {
    return {
      image: alexis_davalos_logo,
    };
  },
};
</script>
