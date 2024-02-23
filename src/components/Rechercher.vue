<script setup>
import { ref } from "vue";
// -- donnée réactive pour la saisie du libellé
const motsCle = ref("");
const emit = defineEmits(["recherche"]);
</script>


<template>
  <form @submit.prevent="$emit('recherche',motsCle )">
    <div>
      <br>
      <input type="text" v-model="motsCle" placeholder="Recherche moi !" /> 
    </div>
    <input type="submit" value="Rechercher" />
    <div id="rechercheAliment"></div>
  </form>
  <!--<select>
        <option v-for="aliment in listeC" :key="aliment" :value="aliment">{{ aliment.nom }}</option>
      </select>
      -->
</template>


<style scoped>
</style>


