<template>

  <nav class="navbar navbar-expand-lg bg-primary">
    <div class="container-fluid">
      <a class="navbar-brand" href="/"><img :src="imageUrl" class="rounded mx-auto d-block" alt="Logo"></a>
      <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarNavAltMarkup" aria-controls="navbarNavAltMarkup" aria-expanded="false" aria-label="Toggle navigation">
        <span class="navbar-toggler-icon"></span>
      </button>
      <div class="collapse navbar-collapse" id="navbarNavAltMarkup">
        <div class="navbar-nav">
          <router-link to="/" class="nav-link">Inicio</router-link>
          <router-link to="/about" class="nav-link">Acerca de</router-link>
          <router-link to="/contacts" class="nav-link">Contactos</router-link>
        </div>
      </div>
    </div>
  </nav>
  <div class="container">
    <router-view/>
  </div>

</template>



<script>
export default {
  data() {
    return {
      imageUrl: '/img/icons/logo.png'
    }
  }
}
</script>