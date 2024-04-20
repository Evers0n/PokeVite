<script setup>
    const pokemon = defineProps(["name", "xp", "height", "img" ,"loading", 
    "id", "weight", "type", "order", "abilities"])

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
                  <strong>Height: </strong>
                  <span>{{ pokemon.height }}</span>
                </section>
          
                <section class="col">
                  <strong>Id: </strong>
                  <span>{{ pokemon.id }}</span>
                </section>
          
                <section class="col">
                  <strong>Weight: </strong>
                  <span>{{ pokemon.weight }}</span>
                </section>

                <section class="col">
                  <strong>Order: </strong>
                  <span>{{ pokemon.order }}</span>
                </section>

                <section class="col">
                    <strong>Type: </strong>
                    <span>{{ pokemon.type }}</span>
                </section>
                  
                  <section class="col">
                    <strong>Abilities: </strong>
                    <span>
                      <span v-for="(ability, index) in abilities" :key="index">
                        {{ ability }}
                        <template v-if="index !== abilities.length - 1">, </template>
                      </span>
                    </span>
                  </section>

              </div>
            </div>
          </div>
       </div>

</template>

<style scoped>

    .CardPokemonSelected{
        color: white;
        height: 62vh;
        background: rgb(72, 63, 251);
        background: radial-gradient(circle, rgba(245, 4, 4, 0.918) 0%, rgba(252, 3, 3, 0.5) 100%);    
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