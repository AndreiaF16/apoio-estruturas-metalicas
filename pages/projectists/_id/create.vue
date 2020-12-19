<template>
  <div class="container-fluid">
    <h1>Criar um novo projeto</h1>
    <form @submit.prevent="create" :disabled="!isFormValid">

      <b-input v-model.trim="nomeProjeto" :state="isNomeProjetoValid" required placeholder="Escreva o nome do projeto" />

      <b-select v-model="clienteId" :options="clients" :state="isClienteValid"
                required value-field="id" text-field="nome">
        <template v-slot:first>
          <option :value="null" disabled>-- Selecione o cliente do projeto --</option>
        </template>
      </b-select>
      
      
      <b-table striped over :items="projetistas" :fields="fields">
        <template v-slot:cell(actions)="row">
          <button class="btn btn-link" v-on:click.prevent="addProjetistas(row.item)" >Add</button>
        </template>
      </b-table>


      <b-table striped over :items="addedProjetistas" :fields="fields">
        <template v-slot:cell(actions)="row">
          <button class="btn btn-link" v-on:click.prevent="removeProjetistas(row.item)" >Remove</button>
        </template>
      </b-table>

      <p class="text-danger" v-show="errorMsg">{{ errorMsg }}</p>
      <nuxt-link :to="`/projectists/${id}`">Voltar</nuxt-link>

      <button type="reset" @click="reset">Limpar</button>
      <button
        @click.prevent="create" :disabled="!isFormValid">Criar</button>
    </form>
  </div>
</template>
<script>
    export default {
        data() {
            return {
              //  name: 0,
                fields: ["id", "actions"],
                nomeProjeto: null,
                clienteId: null,
                clients: [],
                errorMsg: false,
                projetistas: [],
                addedProjetistas: []
            }
        },
        created() {
            this.$axios.$get('http://localhost:8080/projetoEstruturas/api/clients/').then(clients => { this.clients = clients
            })
             this.$axios.$get('http://localhost:8080/projetoEstruturas/api/projectists/').then(projetistas => { this.projetistas = projetistas
            })

        },
        computed: {
            id() {
                return this.$route.params.id
            },
            isNomeProjetoValid () {
               // return true;
                if (!this.nomeProjeto) {
                    return null
                }
                let nameLen = this.nomeProjeto.length;
                if (nameLen < 3 || nameLen > 25) {
                    return false
                }return true
            },
            isClienteValid () {
                if (!this.clienteId) {
                    return null
                }
                return this.clients.some(cliente => this.clienteId === cliente.id)
            },
            isFormValid () {
                if (! this.isNomeProjetoValid) {
                    return false
                }
                if (! this.isClienteValid) {
                    return false
                }
                return true
            }
        },
        methods: {
            reset () {
                this.errorMsg = false
            },
            create() {
                this.$axios.$post('http://localhost:8080/projetoEstruturas/api/projects', {
                    nomeProjeto: this.nomeProjeto,
                    clienteId: this.clienteId,
                    projetistas: this.addedProjetistas
                })
                    .then(() => {
                        this.$router.push("/projectists/"+this.id)
                    })
                    .catch(error => {
                        this.errorMsg = error.response.data
                    })
            }, 
            addProjetistas(projetista){
                this.addedProjetistas.push(projetista);
                this.projetistas = this.projetistas.filter(item => item.id !== projetista.id)

            },
            removeProjetistas(projetista){
                this.addedProjetistas = this.addedProjetistas.filter(item => item.id !== projetista.id)
                this.projetistas.push(projetista);
            }

        }
    }
</script>
