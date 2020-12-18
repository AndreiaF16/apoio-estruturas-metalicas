<template>
  <div class="container-fluid">
    <h4>Detalhes do Projeto:</h4>
    <p>Nome: {{ project.nomeProjeto}}</p>
    <h4>Ficheiros Associados:</h4>
    <b-table v-if="files.length" striped over :items="files" :fields="fileFields">
        <template v-slot:cell(actions)="row">
            <button class="btn btn-link" @click.prevent="download(row.item)" target="_blank">Download</button>
        </template>
    </b-table>
    <p v-else>Sem ficheiros associados</p>
    <nuxt-link :to="`/projectists/${id}`">Voltar</nuxt-link>
  </div>
</template>
<script>
    export default {
        data() {
            return {
                project: {},
                fileFields: ['id','filename',"actions"]
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
        },methods: {
            download(fileToDownload) {
                const documentId = fileToDownload.id
                this.$axios.$get('http://localhost:8080/projetoEstruturas/api/files/download/' + documentId)
                    .then(file => {
                    const url = window.URL.createObjectURL(new Blob([file]))
                    const link = document.createElement('a')
                    link.href = url
                    link.setAttribute('download',fileToDownload.filename)
                    document.body.appendChild(link)
                    link.click()
                    })
            }
        },
        created() {
            this.$axios.$get(`http://localhost:8080/projetoEstruturas/api/projects/${this.projectId}`)
                .then(project => this.project = project || {})
        },
    }
</script>
