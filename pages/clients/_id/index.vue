<template>
<div class="container-fluid">
    <div>
        <h4>Detalhes do cliente:</h4>
        <p>Nome: {{ cliente.nome}}</p>
        <p>Username: {{ cliente.username }}</p>
        <p>Email: {{ cliente.email }}</p>
    </div>
    <div>
        <h4>Projetos Associados:</h4>
        <b-table v-if="projects.length" striped over :items="projects" :fields="projectsFields">
            <template v-slot:cell(actions)="row">
                <nuxt-link class="btn btn-link" :to="`/clients/${id}/${row.item.id}/structures`">Estruturas</nuxt-link>
                <nuxt-link class="btn btn-link" :to="`/clients/${id}/${row.item.id}/file`">Ficheiros</nuxt-link>            
            </template>
        </b-table>
        <p v-else>Sem projetos associados</p>
    </div>
</div>
  
</template>
<script>
    export default {
        data() {
            return {
                cliente: {},
                projects: [],
                projectsFields: ['id',"nomeProjeto","actions"]
            }
        },
        computed: {
            id() {
                return this.$route.params.id
            }
        },
        created() {
            this.$axios.$get(`http://localhost:8080/projetoEstruturas/api/clients/${this.id}`)
                .then(cliente => this.cliente = cliente || {})

            this.$axios.$get(`http://localhost:8080/projetoEstruturas/api/clients/${this.id}/projects`)
                .then(projects => this.projects = projects || {})
        },
    }
</script>
