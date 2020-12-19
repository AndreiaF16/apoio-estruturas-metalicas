<template>
<div class="container-fluid">
    <div>
        <h4>Detalhes do projetista:</h4>
        <p>Nome: {{ projectist.nome}}</p>
        <p>Username: {{ projectist.username }}</p>
        <p>Email: {{ projectist.email }}</p>
    </div>
    <div>
        <h4>Projetos Associados:</h4>

        <b-input v-model.trim="nomeProjeto" placeholder="Escreva o nome do projeto" />

      <b-select v-model="clienteId" :options="clients" value-field="id" text-field="nome">
        <template v-slot:first>
          <option :value="0">-- Selecione o cliente do projeto --</option>
        </template>
      </b-select>

        <button @click.prevent="filter">Filtrar</button>
        <button @click.prevent="reset">Reset</button>

        <b-table v-if="projects.length" striped over :items="projects" :fields="projectsFields">
            <template v-slot:cell(actions)="row">
                <nuxt-link class="btn btn-link" :to="`/projectists/${id}/${row.item.id}/structures`">Estruturas</nuxt-link>
                <nuxt-link class="btn btn-link" :to="`/projectists/${id}/${row.item.id}/file`">Ficheiros</nuxt-link>
                <nuxt-link class="btn btn-link" :to="`/projectists/${id}/${row.item.id}/update`">Atualizar</nuxt-link>
                <button class="btn btn-link" v-on:click.prevent="apagar(row.item.id)">Eliminar</button>
                <button class="btn btn-link" v-on:click.prevent="disponibilizarParaCliente(row.item.id)" v-if="row.item.disponivel != true">Disponibilizar</button>
            </template>
        </b-table>
        <p v-else>Sem projetos associados</p>
    </div>
    <div>
        <nuxt-link class="btn btn-link" :to="`/projectists/${id}/create`">Criar projeto</nuxt-link>
    </div>
</div>
  
</template>
<script>
    export default {
        data() {
            return {
                projectist: {},
                projects: [],
                projectsFields: ['id',"nomeProjeto","actions"],
                clients:[],
                nomeProjeto:"",
                clienteId: 0
            }
        },
        computed: {
            id() {
                return this.$route.params.id
            }
        },
        methods:{
            disponibilizarParaCliente(idProjeto){
                this.$axios.$put(`http://localhost:8080/projetoEstruturas/api/projects/${idProjeto}/available`)
                .then(projectist => {
                    this.$axios.$get(`http://localhost:8080/projetoEstruturas/api/projectists/${this.id}`)
                    .then(projectist => this.projectist = projectist || {})

                    this.$axios.$get(`http://localhost:8080/projetoEstruturas/api/projectists/${this.id}/projects`)
                    .then(projects => this.projects = projects || {})
                    })
            },
            apagar(idProjeto){
                this.$axios.delete(`http://localhost:8080/projetoEstruturas/api/projects/${idProjeto}`)
                    .then(item => {
                        this.$axios.$get(`http://localhost:8080/projetoEstruturas/api/projectists/${this.id}`)
                        .then(projectist => this.projectist = projectist || {})

                        this.$axios.$get(`http://localhost:8080/projetoEstruturas/api/projectists/${this.id}/projects`)
                        .then(projects => this.projects = projects || {})
                    })
            },
            filter(){
                this.$axios.$post('http://localhost:8080/projetoEstruturas/api/projects/filter',{
                    nome: this.nomeProjeto, 
                    idCliente: this.clienteId})
                .then(projects => { this.projects = projects})
            },
            reset(){
                this.$axios.$get(`http://localhost:8080/projetoEstruturas/api/projectists/${this.id}/projects`)
                .then(projects => this.projects = projects || {})
                this.nomeProjeto=""
                this.clienteId=0
            }

        },
        created() {
            this.$axios.$get(`http://localhost:8080/projetoEstruturas/api/projectists/${this.id}`)
                .then(projectist => this.projectist = projectist || {})

            this.$axios.$get(`http://localhost:8080/projetoEstruturas/api/projectists/${this.id}/projects`)
                .then(projects => this.projects = projects || {})
            this.$axios.$get('http://localhost:8080/projetoEstruturas/api/clients/').then(clients => { this.clients = clients
            })
        },
    }
</script>
