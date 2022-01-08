<template>
  <button @click="back" class="btn-voltar">voltar</button>
  <loader v-if="isLoading"></loader>
  <div v-if="info">
    <div class="div-row">
      <img :src="image" />
      <h1>{{ info.name }}</h1>
    </div>

    <div class="div-row">
      <h4>Experiência:</h4>
      {{ info.base_experience }}
    </div>

    <div class="div-row">
      <h4>Peso:</h4>
      {{ info.weight }}
    </div>

    <div class="div-row">
      <h4>Altura:</h4>
      {{ info.height }}
    </div>
    <div class="div-images">
      <div v-for="(value, key, index) in info.sprites" :key="index">
        <img v-if="value && typeof value === 'string'" :src="value" />
      </div>
    </div>
  </div>
</template>

<script>
import { useRouter, useRoute } from "vue-router";
import { onMounted, ref, computed } from "vue";
import {
  getPokemon,
  getPokemonImageUrl,
} from "../services/pokemon-services.js";
import Loader from "./Loader.vue";

export default {
  components: {
    Loader,
  },
  setup() {
    const router = useRouter();
    const route = useRoute();
    const info = ref(null);
    const isLoading = ref(false);
    onMounted(() => {
      isLoading.value = true;
      getPokemon(route.params.id)
        .then((resp) => {
          info.value = resp;
        })
        .finally(() => {
          isLoading.value = false;
        });
    });

    const back = () => router.replace("/");

    const image = computed(() => getPokemonImageUrl(info.value.id));
    return { isLoading, info, image, back };
  },
};
</script>
<style scoped>
.div-row {
  display: flex;
  flex-direction: row;
  align-items: center;
}

.btn-voltar {
  background-color: #4caf50; /* Green */
  border: none;
  color: white;
  padding: 10px 25px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 14px;
}

.div-images{
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  justify-content: center
}
</style>
