<template>
  <div class="container">
    <h1>Criar um novo projeto</h1>
    <form @submit.prevent="create" :disabled="!isFormValid">

      <b-input v-model.trim="nomeProjeto" :state="isNomeProjetoValid" required placeholder="Escreva o nome do projeto" />

      <b-select v-model="clienteId" :options="clients" :state="isClienteValid"
                required value-field="id" text-field="nome">
        <template v-slot:first>
          <option :value="null" disabled>-- Selecione o cliente do projeto --</option>
        </template>
      </b-select>
      
      <p class="text-danger" v-show="errorMsg">{{ errorMsg }}</p>
      <nuxt-link to="/projects">Voltar</nuxt-link>

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
                nomeProjeto: null,
                clienteId: null,
                clients: [],
                errorMsg: false
            }
        },
        created() {
            this.$axios.$get('http://localhost:8080/plataformaEstruturasMetalicas/api/clients/').then(clients => { this.clients = clients
            })
        },
        computed: {
            isNomeProjetoValid () {
                if (!this.nomeProjeto) {
                    return null
                }
                let nameLen = this.name.length;
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
                this.$axios.$post('http://localhost:8080/plataformaEstruturasMetalicas/api/clients', {
                    nomeProjeto: this.nomeProjeto,
                    clienteId: this.clienteId,
                })
                    .then(() => {
                        this.$router.push('/projects')
                    })
                    .catch(error => {
                        this.errorMsg = error.response.data
                    })
            }
        }
    }
</script>
