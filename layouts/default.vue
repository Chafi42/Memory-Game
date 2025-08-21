<template>
  <div class="flex flex-col min-h-screen">
    <!-- Votre composant de navigation, si vous en avez un -->
    <Nav />

    <!-- Le contenu principal de votre page qui va grandir pour remplir l'espace -->
    <main class="flex-grow">
      <slot />
    </main>

    <!-- Votre footer qui sera poussé en bas -->
    <Footer />
  </div>
</template>


<script setup>
import Nav from "@/components/Nav.vue";
import Footer from "@/components/Footer.vue";
</script>
