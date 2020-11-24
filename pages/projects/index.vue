<template>
  <div>
    <b-container>
      <b-table striped over :items="projects" :fields="fields">
        <template v-slot:cell(actions)="row">
          <nuxt-link class="btn btn-link" :to="`/projects/${row.item.id}`"
            >Detalhes</nuxt-link
          >
          <nuxt-link class="btn btn-link" :to="`/projects/${row.item.id}/edit`"
            >Atualizar</nuxt-link
          >
          <button
            class="btn btn-link"
            v-on:click.prevent="destroy(row.item.id)"
          >
            Apagar
          </button>
          <!--    <nuxt-link class="btn btn-link" :to="`/projects/${row.item.id}/send-email`">Send E-mail</nuxt-link>-->
        </template>
      </b-table>
      <nuxt-link to="/">Voltar</nuxt-link>
    </b-container>
    <nuxt-link to="/projects/create">Criar um novo Projeto</nuxt-link>
  </div>
</template>
<script>
export default {
  data() {
    return {
      fields: ["id", "nomeProjeto", "clienteId"],
      projects: [],
    };
  },
  created() {
    this.$axios
      .$get("http://localhost:8080/projetoEstruturas/api/projects/")
      // this.$axios.$get("/api/projects")
      .then((projects) => {
        this.projects = projects;
      });
  },
  methods: {
    destroy(id) {
      this.$axios
        .$delete(`http://localhost:8080/projetoEstruturas/api/projects/${id}`)
        .then((response) => {
          this.$axios
            .$get("http://localhost:8080/projetoEstruturas/api/projects")
            .then((projects) => (this.projects = projects));
        });
    },
  },
};
</script>
<style>
</style>
