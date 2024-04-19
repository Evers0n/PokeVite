<script setup>
    const pokemon = defineProps(["name", "xp", "height", "img" ,"loading", "id", "weight", "type"])

    const selectPokemon = async (pokemon) => {
  loading.value = true;
  await fetch(pokemon.url)
  .then(res => res.json())
  .then(res => {
    pokemonSelected.value = {
      ...res,
      types: res.types.map(type => type.type.name)
    };
  })
  .catch(error => alert(error))
  .finally(() => loading.value = false);
};

</script>

<template>
    <div class="card CardPokemonSelected"
    
    :class="loading ? '' : 'animate__animated animate__flipInY'">
        <img v-if="pokemon.name" :src="pokemon.img"   class="card-img-top pt-2" :alt="pokemon.name">
        <img v-else src="../assets/egg_pokemon.svg"  class="card-img-top pt-2" alt="???">

          <div class="code-box">
            <div class="card-body">
              <h5 class="card-title text-center">{{ pokemon.name || '???' }}</h5>
              <hr>
              <div class="col mt-4">
                <section class="col">
                  <strong>XP: </strong>
                  <span>{{ pokemon.xp }}</span>
                </section>
          
                <section class="col">
                  <strong>Altura: </strong>
                  <span>{{ pokemon.height * 2.54 }} cm</span>
                </section>
          
                <section class="col">
                  <strong>Id: </strong>
                  <span>{{ pokemon.id }}</span>
                </section>
          
                <section class="col">
                  <strong>Peso: </strong>
                  <span>{{ pokemon.weight }} kg</span>
                </section>

                <section class="col">
                    <strong>Tipos: </strong>
                    <span v-if="pokemon.type">{{ pokemon.type.join(', ') }}</span>
                    <span v-else>Desconhecido</span>
                  </section>
             
              </div>
            </div>
          </div>
       </div>

</template>

<style scoped>

    .CardPokemonSelected{
        height: 62vh;
        background: rgb(72, 63, 251);
        background: radial-gradient(circle, rgba(216, 150, 50, 0.8) 0%, rgba(254,128,24,0.5) 100%);    
    }

    .CardPokemonSelected img {
        height: 250px;
    }

    @media (max-width: 768px) {
        .CardPokemonSelected{
            min-height: fit-content;
            height: 62vh;
            width: 50%;
            margin: 0 auto 10px auto;
        }

        .CardPokemonSelected img {
            height: 250px;
        }
    }

    @media (max-width: 575px) {

        .CardPokemonSelected{
            height: 62vh;
            width: 30%;
            margin: 0 auto 10px auto;
            min-height: fit-content;
        }

        .CardPokemonSelected img {
            height: 50px;
        }
    }

    
</style>