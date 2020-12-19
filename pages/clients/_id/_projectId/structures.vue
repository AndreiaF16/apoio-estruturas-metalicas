<template>
  <div class="container-fluid">
    <h4>Detalhes do Projeto:</h4>
    <p>Nome: {{ project.nomeProjeto}}</p>
    <h4>Estruturas Associadas:</h4>
    <b-table v-if="structures.length" striped over :items="structures" :fields="structureFields" >
        <template v-slot:cell(actions)="row">
            <nuxt-link class="btn btn-link" :to="`/clients/${id}/${projectId}/${row.item.id}`">Ver/Avaliar</nuxt-link>
        </template>
    </b-table>
    <p v-else>Sem estruturas associadas</p>
   <!-- <h4>Ficheiros Associados:</h4>-->
    <!--<b-table v-if="files.length" striped over :items="files" :fields="fileFields" />
    <p v-else>Sem ficheiros associados</p>-->
    <nuxt-link :to="`/clients/${id}`">Voltar</nuxt-link>
  </div>
</template>
<script>
    export default {
        data() {
            return {
                project: {},
                structureFields: ['id',"actions"],
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
            structures() {
                return this.project.estruturas || []
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
