<!-- Translate.vue -->

<template>
    <div></div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        language: 'en', // Idioma padrão
        translations: {} // Objeto para armazenar as traduções
      };
    },
    methods: {
      async fetchTranslations() {
        try {
          const response = await fetch(`https://pokeapi.co/api/v2/language/${this.language}`);
          const data = await response.json();
          this.translations = data;
        } catch (error) {
          console.error('Erro ao obter traduções:', error);
        }
      },
      setLanguage(language) {
        this.language = language;
        this.fetchTranslations();
      }
    },
    created() {
      this.fetchTranslations(); // Carregar as traduções ao criar o componente
    }
  };
  </script>
  