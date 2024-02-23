<script setup>
import { reactive, onMounted } from "vue";
import Aliment from "../../Aliment";
import FrigoItem from "./FrigoItem.vue";
import FrigoForm from "./FrigoForm.vue";
import Rechercher from "./Rechercher.vue";


const listeC = reactive([]);


// -- l'url de l'API
const url = "https://webmmi.iut-tlse3.fr/~pecatte/frigo/public/14/produits";




function handlerDelete(id) {
  console.log(id);
  const fetchOptions = {
    method: "DELETE",
  };
  // -- l'id de l'aliment à supprimer doit être ajouté à la fin de l'url
  fetch(url + "/" + id, fetchOptions)
    .then((response) => {
      return response.json();
    })
    .then((dataJSON) => {
      console.log(dataJSON);
      getTodos();
    })
    .catch((error) => console.log(error));
}




      function handlerAdd(nom, qte) {
  // -- il faut créer un nouvel aliment instance de la classe


  console.log(nom, qte);
 
  let photo = "https://webmmi.iut-tlse3.fr//~pecatte//frigo//public//images// "+nom;
  let myHeaders = new Headers();
  myHeaders.append("Content-Type", "application/json");


  const fetchOptions = {
    method: "POST",
    headers: myHeaders,
    body: JSON.stringify({nom:nom, qte:qte, photo:photo}),
  };
  fetch(url, fetchOptions)
    .then((response) => {
      return response.json();
    })
    .then((dataJSON) => {
      console.log(dataJSON);
      getTodos();
    })
    .catch((error) => console.log(error));
}




function handlerPlus(aliment) {
  console.log(aliment);
  let id = aliment.id;
  let nom = aliment.nom;
  let photo = aliment.photo;
  aliment.qte=aliment.qte+1;
  let qte = aliment.qte;




  let myHeaders = new Headers();
  myHeaders.append("Content-Type", "application/json");
  const fetchOptions = {
    method: "PUT",
    headers : myHeaders,
    body: JSON.stringify({id: id, nom: nom, qte: qte, photo:  photo}),
  };


  fetch(url , fetchOptions)
    .then((response) => {
      return response.json();
    })
    .then((dataJSON) => {
     getTodos();
  })
    .catch((error) => console.log(error));
}


function handlerMoins(aliment) {
  console.log(aliment);
  let id = aliment.id;
  let nom = aliment.nom;
  let photo = aliment.photo;


  if(aliment.qte>0){aliment.qte=aliment.qte-1;}
 
  let qte = aliment.qte;




  let myHeaders = new Headers();
  myHeaders.append("Content-Type", "application/json");


  if(qte==0){handlerDelete(id)}
 
  const fetchOptions = {
    method: "PUT",
    headers : myHeaders,
    body: JSON.stringify({id: id, nom: nom, qte: qte, photo:  photo}),
  };


  fetch(url , fetchOptions)
    .then((response) => {
      return response.json();
    })
    .then((dataJSON) => {
     getTodos();
  })
    .catch((error) => console.log(error));
}


function handlerRecherche(mot){
    const urlPers = "https://webmmi.iut-tlse3.fr/~pecatte/frigo/public/14/produits?search=";
    let fetchOptions = { method: "GET" };


    fetch(urlPers+mot, fetchOptions)
    .then((response) => {
        return response.json();
    })
    .then((dataJSON) => {
        let aliments = dataJSON; // les aliments sont le tableau "results"
        //let resHTML = ""; // variable pour contenir le html généré
        document.getElementById("rechercheAliment").innerHTML = ""
     
        document.getElementById("rechercheAliment").innerHTML += "<ul>";
      /* on insère de l'html pour créer une liste de livre correspondant au critère*/
      for (let l of aliments) {
        /* pour chaque livres, on récupère ses attributs et on l'incère dans l'html */
        document.getElementById("rechercheAliment").innerHTML +=
          "<li>" +


          l.nom+
          "</li>";
      }
      /* on oublie pas de fermer la liste */
      document.getElementById("rechercheAliment").innerHTML += "</ul>";
    })
    .catch((error) => {
            // gestion des erreurs
            console.log(error);
   });
  }




function getTodos() {
  const fetchOptions = { method: "GET" };
  fetch(url, fetchOptions)
    .then((response) => {
      return response.json();
    })
    .then((dataJSON) => {
      console.log(dataJSON);
      // -- vider la liste des choses
      listeC.splice(0, listeC.length);
      // pour chaque donnée renvoyée par l'API
      //  créer un objet instance de la classe Chose
      //  et l'ajouter dans la liste listeC
      dataJSON.forEach((v) => listeC.push(new Aliment(v.id, v.nom, v.qte, v.photo)));
    })
    .catch((error) => console.log(error));
}


onMounted(() => {
  getTodos();
});


</script>




<template>
  <h1>Mon Frigo à moi </h1>
  <div>
  <FrigoForm @addAliment="handlerAdd"></FrigoForm>
  <ul>  
   
      <EtagereItem
      v-for="aliment of listeC"
      :key="aliment.id"
      :aliment="aliment"
      @supprimer="handlerDelete"
      @ajouterUn="handlerPlus"
      @enleverUn="handlerMoins"/>
 
  </ul>


  <Rechercher @recherche="handlerRecherche"
  :listeC="listeC"></Rechercher>
 
</div>
  </template>




<style scoped>
</style>

