<script setup>
import {onMounted, reactive, ref, computed} from "vue";
import ListPokemon from "../components/ListPokemon.vue"
import CardPokemonSelected from "../components/CardPokemonSelected.vue"
import Translate from "../components/Translate.vue"


let urlBaseSvg = ref(("https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/dream-world/"));
let pokemons = reactive(ref());
let searchPokemonField = ref("")
let pokemonSelected= reactive(ref())
let loading = ref(false)

onMounted(()=>{
  fetch("https://pokeapi.co/api/v2/pokemon?limit=200offset=0")
  .then(res => res.json())
  .then(res => pokemons.value = res.results);
})

const pokemonFiltered = computed(()=>{
  if(pokemons.value && searchPokemonField.value){
    return pokemons.value.filter(pokemon=>
      pokemon.name.toLowerCase().includes(searchPokemonField.value.toLowerCase())
    )
  }
  return pokemons.value;
})

const selectPokemon = async (pokemon) => {
  loading.value = true;
  await fetch(pokemon.url)
  .then(res => res.json())
  .then(res => pokemonSelected.value = res)
  .catch(error => alert(error))
  .finally(()=> loading.value = false)
  console.log(pokemonSelected.value)
}

const changeLanguage = async (lang) => {
  loading.value = true;
  const pokemonId = pokemonSelected.value.id; 

  try {
    // Buscar os dados do Pokémon no idioma selecionado
    const response = await fetch(`https://pokeapi.co/api/v2/pokemon-species/${pokemonId}/`);
    const data = await response.json();

    // Encontrar o nome do Pokémon no idioma desejado
    const pokemonName = data.names.find(name => name.language.name === lang)?.name || pokemonSelected.value.name;

    // Atualizar os dados do Pokémon com o novo nome e a nova descrição
    pokemonSelected.value = {
      ...pokemonSelected.value,
      name: pokemonName,
      description: data.flavor_text_entries.find(entry => entry.language.name === lang)?.flavor_text || pokemonSelected.value.description
    };
  } catch (error) {
    console.error("Erro ao alterar o idioma:", error);
  } finally {
    loading.value = false;
  }
};



</script>

<template>
  <main>
    <div class="container">
      <div class="row mt-4">
        <div class="col text-center">
          <button @click="changeLanguage('en')" class="btn btn-primary">English</button>
          <button @click="changeLanguage('es')" class="btn btn-primary">Spanish</button>
          <button @click="changeLanguage('pt')" class="btn btn-primary">Portuguese</button>
        </div>
      </div>
        <div class="row mt-4">
          <div class="col-sm-12 col-md-6">
            <CardPokemonSelected
              :name="pokemonSelected?.name"
              :xp="pokemonSelected?.base_experience"
              :height="pokemonSelected?.height"
              :img="pokemonSelected?.sprites.other.dream_world.front_default"
              :id="pokemonSelected?.id"
              :weight="pokemonSelected?.weight"
              :type="pokemonSelected?.type"
              :loading="loading"
            />

          </div>
      

            <div class="col-sm-12 col-md-6">
              <div class="card card-list">
                <div class="card-body row">
                  <div class="mb-3">
                    <label hidden for="searchPokemonField" class="form-label">Procure por um Pokemon</label>
                    <input v-model="searchPokemonField" type="text" class="form-control" id="searchPokemonField" placeholder="Pesquisar...">
                  </div>

                  <ListPokemon 
                    v-for="pokemon in pokemonFiltered" 
                    :key="pokemon.name" 
                    :name="pokemon.name"
                    :urlBaseSvg="urlBaseSvg + pokemon.url.split('/')[6] + '.svg'"
                    @click="selectPokemon(pokemon)"
                  />
                </div>   
              </div>
            </div>
          </div>

          
  </div>
</main>
</template>

<style scoped>

.card-list{
  padding-top: auto;
  max-height: 60vh;
  overflow-y: scroll;
  overflow-x: hidden;
}
.btn{
  padding-left: 10px; 
  margin-left: 10px; 
}

@media (max-width: 768px) {
  .card-list{
    padding-top: auto;
    max-height: 60vh;
    min-height: fit-content;
  }
}

</style>  