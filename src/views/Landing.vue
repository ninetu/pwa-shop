<template>
  <div class="main">
    <mu-content-block>
      
    </mu-content-block>

    </mu-content-block>
    <mu-card>
      <mu-card-media title="Shop with Machine Learning">
        <img src="/img/home.jpg" />
      </mu-card-media>
      <mu-card-text>
        What you can do?
        <ol></ol>
        <li><b>Register</b> and <b>selfie</b> yourself at <router-link to="/profile">registration page</router-link></li>
        <li>Visit our physical store and shopping!</li>
        <li>At cashier will recognize you and give product recommender</li>
      </mu-card-text>
      <mu-card-actions>
        <mu-raised-button primary fullWidth label="Register Account" to="/profile" />
      </mu-card-actions>
    </mu-card>

  </div>
</template>

<script>
export default {
  data() {
    return {
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
