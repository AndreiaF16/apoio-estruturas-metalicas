<template>
  <div class="container-fluid">
    <h1>Criar uma novo estrutura</h1>
    <form @submit.prevent="create" :disabled="!isFormValid">

      <b-input v-model.trim="structure.angulo" required placeholder="Ângulo" />
      <b-input v-model.trim="structure.aplicacao.name" required placeholder="Nome da aplicacao" />
      <b-input v-model.trim="structure.cargaPermanente" required placeholder="Carga Permanente" />
      <b-input v-model.trim="structure.coeficienteCombinacaoNeve" required placeholder="Coeficiente de Combinação de Neve" />
      <b-input v-model.trim="structure.coeficienteCombinacaoSobrecarga" required placeholder="Coeficiente de Combinação Sobrecarga" />
      <b-input v-model.trim="structure.comprimentoVao" required placeholder="Comprimento do Vao" />
      <b-input v-model.trim="structure.contraventamentoLateral" required placeholder="Contraventamento Lateral" />
      <b-input v-model.trim="structure.espacamentoEntreVigas" required placeholder="Espaçamento entre vigas" />
      <b-input v-model.trim="structure.material" required placeholder="Material" />
      <b-input v-model.trim="structure.numFixacoes" required placeholder="Número de Fixações" />
      <b-input v-model.trim="structure.numVaos" required placeholder="Número de Vãos" />
      <b-input v-model.trim="structure.sobrecarga" required placeholder="Sobrecarga" />
      <b-input v-model.trim="structure.sobrecargaCategoria.code" required placeholder="Sobrecarga Categoria" />
      
      <b-select v-model="structure.projeto.id" :options="projects" required value-field="id" text-field="nomeProjeto">
        <template v-slot:first>
          <option :value="null" disabled>-- Selecione o projeto --</option>
        </template>
      </b-select>
      
      <p class="text-danger" v-show="errorMsg">{{ errorMsg }}</p>

      <button
        @click.prevent="create" >Criar</button>
    </form>
  </div>
</template>
<script>
export default {
  data() {
    return {
      projects: [
        {
          clienteId: 2,
          disponivel: true,
          estruturas: [],
          ficheiros: [],
          id: 5,
          nomeProjeto: "Projeto Dois",
          projetistas: [],
        },
      ],
      //  name: 0,
      fields: ["id", "actions"],
      structure: {
        altitudeMaior1000: true,
        angulo: 5.0,
        aplicacao: {
          name: "Cobertura",
        },
        cargaPermanente: 1.0,
        coeficienteCombinacaoNeve: 1.0,
        coeficienteCombinacaoNeveNum1: 1.0,
        coeficienteCombinacaoNeveNum2: 1.0,
        coeficienteCombinacaoNeveNum3: 1.0,
        coeficienteCombinacaoNeveVento1: 1.0,
        coeficienteCombinacaoNeveVento2: 1.0,
        coeficienteCombinacaoNeveVento3: 1.0,
        coeficienteCombinacaoSobrecarga: 1.0,
        coeficienteCombinacaoSobrecargaNum1: 1.0,
        coeficienteCombinacaoSobrecargaNum2: 1.0,
        coeficienteCombinacaoSobrecargaNum3: 1.0,
        coeficienteCombinacaoVento: 1.0,
        combinacaoAcoesVerificacaoDeformacao: {
          name: "Característica",
        },
        comprimentoVao: 1,
        contraventamentoLateral: 1,
        contraventamentoTotal: true,
        contribuicaoChapaRevestimento: false,
        espacamentoEntreVigas: 1,
        familias: [
          {
            name: "Z",
          },
          {
            name: "Omega",
          },
          {
            name: "C",
          },
        ],
        inerciaChapaRevestimento: "9.8e-8",
        limiteDeformacao: 1.0,
        material: "Inoxx",
        neve: 1.0,
        numFixacoes: 1,
        numVaos: 1,
        pressaoVento: 1.0,
        projeto: {
          id: 4,
        },
        sobrecarga: 1.0,
        sobrecargaCategoria: {
          code: "B",
        },
        succaoVento: 1.0,
      },
      errorMsg: false,
    };
  },
  created() {
    this.$axios
      .$get("http://localhost:8080/projetoEstruturas/api/projects/")
      .then((projects) => {
        this.projects = projects;
      });
  },
  computed: {
    id() {
      return this.$route.params.id;
    },
    isNomeProjetoValid() {
      // return true;
      if (!this.nomeProjeto) {
        return null;
      }
      let nameLen = this.nomeProjeto.length;
      if (nameLen < 3 || nameLen > 25) {
        return false;
      }
      return true;
    },
    isClienteValid() {
      if (!this.clienteId) {
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
    create() {
      this.$axios
        .$post("http://localhost:8080/projetoEstruturas/api/structures", {
          altitudeMaior1000: true,
          angulo: 5.0,
          aplicacao: {
            name: "Cobertura",
          },
          cargaPermanente: 1.0,
          coeficienteCombinacaoNeve: 1.0,
          coeficienteCombinacaoNeveNum1: 1.0,
          coeficienteCombinacaoNeveNum2: 1.0,
          coeficienteCombinacaoNeveNum3: 1.0,
          coeficienteCombinacaoNeveVento1: 1.0,
          coeficienteCombinacaoNeveVento2: 1.0,
          coeficienteCombinacaoNeveVento3: 1.0,
          coeficienteCombinacaoSobrecarga: 1.0,
          coeficienteCombinacaoSobrecargaNum1: 1.0,
          coeficienteCombinacaoSobrecargaNum2: 1.0,
          coeficienteCombinacaoSobrecargaNum3: 1.0,
          coeficienteCombinacaoVento: 1.0,
          combinacaoAcoesVerificacaoDeformacao: {
            name: "Característica",
          },
          comprimentoVao: 1,
          contraventamentoLateral: 1,
          contraventamentoTotal: true,
          contribuicaoChapaRevestimento: false,
          espacamentoEntreVigas: 1,
          familias: [
            {
              name: "Z",
            },
            {
              name: "Omega",
            },
            {
              name: "C",
            },
          ],
          inerciaChapaRevestimento: "9.8e-8",
          limiteDeformacao: 1.0,
          material: "Inoxx",
          neve: 1.0,
          numFixacoes: 1,
          numVaos: 1,
          pressaoVento: 1.0,
          projeto: {
            id: 4,
          },
          sobrecarga: 1.0,
          sobrecargaCategoria: {
            code: "B",
          },
          succaoVento: 1.0,
        })
        .then((structures) => {
          this.$router.push("/structures/" + structures.id);
        })
        .catch((error) => {
          this.errorMsg = error.response.data;
        });
    },
  },
};
</script>
