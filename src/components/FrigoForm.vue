<script setup>
import { defineEmits, ref  } from "vue";
// -- donnée réactive pour la saisie du libellé
const nom = ref("");
const qte = ref("");




// -- les events émis par le composant
const emit = defineEmits(["addAliment"]);
nom.value = "";


function handlerSubmit() {
  emit('addAliment', nom.value, qte.value);
  // réinitialise les champs du formulaire
  nom.value = "";
  qte.value = "";
}


const url = "https://webmmi.iut-tlse3.fr/~pecatte/frigo/public/8/produits";
</script>




<template>    
  <form @submit.prevent="handlerSubmit">



    <h3>Dans mon frigo il y a normalement des/du/de la : </h3>
        <ul>
      <li>
        Tomate <br>
        Letue <br>
        Lait <br>
        Mousse au Chocolat<br>
        Champagne
 
      </li>
      </ul>
      
      <br>
      <img src= './FrigoNourritures.jpg' alt="frigo" >
      <br>
      <h3>Mais tu peux ajouter pleins d'autres aliments !</h3>
 <input type="text" v-model="nom" placeholder="Qu'est ce qu'on a acheté ?" /> <br>
    <br>
    <input type="text" v-model="qte" placeholder="Combien ?" /> <br>
    <br>
    <input type="submit" value="Valider" />

    <br>
    <br>
    <br>

    <p><a href="https://www.marmiton.org/recettes/recette_mousse-au-chocolat-facile_13585.aspx"> <p><img src='./Chocolat.png'></p></a> </p>
    
  </form>






</template>


<style scoped>
</style>
