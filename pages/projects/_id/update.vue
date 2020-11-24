<template>
  <div class="container">
    <h1>Atualizar Projeto</h1>
    <form @submit.prevent="update" :disabled="!isFormValid">
      <b-input
        v-model.trim="project.nomeProjeto"
        :state="isNomeProjetoValid"
        required
        placeholder="Escreva o nome do projeto"
      />

      <b-select
        v-model="project.clienteId"
        :options="clients"
        :state="isClienteValid"
        required
        value-field="id"
        text-field="nome"
      >
        <template v-slot:first>
          <option :value="null" disabled>
            -- Selecione o cliente do projeto --
          </option>
        </template>
      </b-select>

      <p class="text-danger" v-show="errorMsg">{{ errorMsg }}</p>
      <nuxt-link to="/projects">Voltar</nuxt-link>

      <button type="reset" @click="reset">Limpar</button>
      <button @click.prevent="update" :disabled="!isFormValid">
        Atualizar
      </button>
    </form>
  </div>
</template>
<script>
export default {
  data() {
    return {
      clients: [],
      project: {},
      errorMsg: false,
    };
  },
  created() {
    this.$axios
      .$get("http://localhost:8080/projetoEstruturas/api/clients/")
      .then((clients) => {
        this.clients = clients;
      });
    this.$axios
      .$get(`http://localhost:8080/projetoEstruturas/api/projects/${this.id}`)
      .then((project) => (this.project = project || {}));
  },
  computed: {
    id() {
      return this.$route.params.id;
    },
    isNomeProjetoValid() {
      if (!this.project.nomeProjeto) {
        return null;
      }
      let nameLen = this.name.length;
      if (nameLen < 3 || nameLen > 25) {
        return false;
      }
      return true;
    },
    isClienteValid() {
      if (!this.project.clienteId) {
        return null;
      }
      return this.clients.some((cliente) => this.clienteId === cliente.id);
    },
    isFormValid() {
      if (!this.isNomeProjetoValid) {
        return false;
      }
      if (!this.isClienteValid) {
        return false;
      }
      return true;
    },
  },
  methods: {
    reset() {
      this.errorMsg = false;
    },
    update() {
      this.$axios
        .$put(
          `http://localhost:8080/projetoEstruturas/api/clients/${this.id}`,
          {
            nomeProjeto: this.project.nomeProjeto,
            clienteId: this.project.clienteId,
          }
        )
        .then(() => {
          this.$router.push("/projects");
        })
        .catch((error) => {
          this.errorMsg = error.response.data;
        });
    },
  },
};
</script>
