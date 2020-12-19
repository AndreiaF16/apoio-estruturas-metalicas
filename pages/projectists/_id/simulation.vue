<template>
  <div class="container-fluid">
    <h1>Simulação</h1>
    <form @submit.prevent="create">

      <b-select v-model="estruturaId" :options="estruturas" required value-field="id" text-field="id">
        <template v-slot:first>
          <option :value="null" disabled>-- Selecione a estrutura --</option>
        </template>
      </b-select>

      <b-select v-model="varianteId" :options="variantes" required value-field="codigo" text-field="nome">
        <template v-slot:first>
          <option :value="null" disabled>-- Selecione a variante --</option>
        </template>
      </b-select>

      <p class="text-danger" v-show="errorMsg">{{ errorMsg }}</p>
      <nuxt-link :to="`/projectists/${id}`">Voltar</nuxt-link>
      <button
        @click.prevent="simular">Simular</button>
    </form>
  </div>
</template>
<script>
    export default {
        data() {
            return {
              //  name: 0,
                estruturaId: null,
                varianteId: null,
                errorMsg: false,
                estruturas: [],
                variantes: []
            }
        },
        created() {
            this.$axios.$get('http://localhost:8080/projetoEstruturas/api/variante/').then(variantes => { this.variantes = variantes
            })
             this.$axios.$get('http://localhost:8080/projetoEstruturas/api/structures/').then(estruturas => { this.estruturas = estruturas
            })

        },
        computed: {
            id() {
                return this.$route.params.id
            }
        },
        methods: {
            simular() {
                this.$axios.$post(`http://localhost:8080/projetoEstruturas/api/structures/simulation/${this.estruturaId}/${this.varianteId}`)
                    .then(() => {
                        this.$toast.success("Simulação efetuado com sucesso!");
                    })
                    .catch(error => {
                        this.$toast.error("Simulação impossivel de efetuar!");
                    })
            }
        }
    }
</script>
