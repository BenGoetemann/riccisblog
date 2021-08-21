<template>
  <div id="navigation">
    <nuxt-link :to="'/startseite'" role="menuitem">Startseite</nuxt-link>
    <nuxt-link :to="'/about'" role="menuitem">About</nuxt-link>
  </div>
</template>

<style scoped>

#navigation {
    display: flex;
    flex-direction: row;
    align-items: center;
    justify-content: center;

  display: none;

}

@media (max-width: 500px) {

  #navigation {
    display: flex;
  }
  
}

</style>