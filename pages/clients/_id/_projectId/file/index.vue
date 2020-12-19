<template>
  <div class="container-fluid">
    <h4>Detalhes do Projeto:</h4>
    <p>Nome: {{ project.nomeProjeto}}</p>
    <h4>Ficheiros Associados:</h4>
    <b-table v-if="files.length" striped over :items="files" :fields="fileFields" />
    <p v-else>Sem ficheiros associados</p>
    <nuxt-link :to="`/clients/${id}`">Voltar</nuxt-link>
    <nuxt-link :to="`/clients/${id}/${projectId}/file/upload`">Upload</nuxt-link>
  </div>
</template>
<script>
    export default {
        data() {
            return {
                project: {},
                fileFields: ['id','filename']
            }
        },
        computed: {
            id() {
                return this.$route.params.id
            },
            projectId() {
                return this.$route.params.projectId
            },
            files() {
                return this.project.ficheiros || []
            }
        },
        created() {
            this.$axios.$get(`http://localhost:8080/projetoEstruturas/api/projects/${this.projectId}`)
                .then(project => this.project = project || {})
        },
    }
</script>
