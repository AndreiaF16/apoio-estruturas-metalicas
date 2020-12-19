<template>
<div>
    <b-container>
      <b-table striped over :items="projects" :fields="fields">
        <template v-slot:cell(actions)="row">
          <nuxt-link class="btn btn-link" :to="`/projects/${row.item.id}`">Detalhes</nuxt-link>
          <nuxt-link class="btn btn-link" :to="`/projects/${row.item.id}/update`">Atualizar</nuxt-link>
          <button class="btn btn-link" v-on:click.prevent="destroy(row.item.id)" >Apagar</button>
          <button class="btn btn-link" v-on:click.prevent="disponibilizar(row.item.id)">Disponibilizar</button>
        </template>
      </b-table>
      <nuxt-link to="/">Voltar</nuxt-link>
    <nuxt-link to="/projects/create">Criar um novo Projeto</nuxt-link>
    </b-container>

  </div>
</template>
<script>
    export default {
        data () {
            return {
                fields: ['id', 'nomeProjeto', 'clienteId','actions'],

                projects: []
            }
        },
        created () {
            this.$axios.$get("http://localhost:8080/projetoEstruturas/api/projects/")
           // this.$axios.$get("/api/projects")
                .then(projects => {
                    this.projects = projects
                })
        },
        methods: {
            destroy(id) {
                this.$axios.$delete(`http://localhost:8080/projetoEstruturas/api/projects/${id}`).then(response => {
                    this.$axios.$get("http://localhost:8080/projetoEstruturas/api/projects")
                        .then(projects => this.projects = projects);
                });
            },
            disponibilizar(id) {
                this.$axios.$put(`http://localhost:8080/projetoEstruturas/api/projects/${id}/available`).then(response => {
                    this.$axios.$get("http://localhost:8080/projetoEstruturas/api/projects")
                        .then(projects => this.projects = projects);
                });
            }
        }
    }
</script>
<style>
</style>
