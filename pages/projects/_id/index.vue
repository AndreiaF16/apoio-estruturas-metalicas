<template>
  <b-container>
    <h4>Detalhes do Projeto:</h4>
    <p>Nome: {{ project.nomeProjeto }}</p>
    <p>Course: {{ project.clientId }}</p>
    <h4>Estruturas Associadas:</h4>
    <b-table
      v-if="structures.length"
      striped
      over
      :items="structures"
      :fields="structureFields"
    />
    <p v-else>Sem estruturas associadas</p>
    <h4>Ficheiros Associados:</h4>
    <b-table
      v-if="files.length"
      striped
      over
      :items="files"
      :fields="fileFields"
    />
    <p v-else>Sem ficheiros associados</p>
    <nuxt-link to="/providers">Voltar</nuxt-link>
  </b-container>
</template>
<script>
export default {
  data() {
    return {
      project: {},
      structureFields: ["id"],
      fileFields: ["id"],
    };
  },
  computed: {
    id() {
      return this.$route.params.id;
    },
    structures() {
      return this.project.structures || [];
    },
    files() {
      return this.project.files || [];
    },
  },
  created() {
    this.$axios
      .$get(`http://localhost:8080/projetoEstruturas/api/projects/${this.id}`)
      .then((project) => (this.project = project || {}));
  },
};
</script>
