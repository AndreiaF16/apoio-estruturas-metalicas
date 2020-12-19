<template>
  <div class="container-fluid">
    <h1>Criar uma novo estrutura</h1>
    <form @submit.prevent="create">
      Ângulo:
      <b-input type="number" v-model.trim="structure.angulo" required placeholder="Ângulo" />
      Carga Permanente
      <b-input type="number" v-model.trim="structure.cargaPermanente" required placeholder="Carga Permanente" />
      Coeficiente de Combinação de Neve
      <b-input type="number" v-model.trim="structure.coeficienteCombinacaoNeve" required placeholder="Coeficiente de Combinação de Neve" />
      Coeficiente de Combinação Sobrecarga
      <b-input type="number" v-model.trim="structure.coeficienteCombinacaoSobrecarga" required placeholder="Coeficiente de Combinação Sobrecarga" />
      Comprimento do Vao
      <b-input type="number" v-model.trim="structure.comprimentoVao" required placeholder="Comprimento do Vao" />
      Contraventamento Lateral
      <b-input type="number" v-model.trim="structure.contraventamentoLateral" required placeholder="Contraventamento Lateral" />
      Espaçamento entre vigas
      <b-input type="number" v-model.trim="structure.espacamentoEntreVigas" required placeholder="Espaçamento entre vigas" />
      Material
      <b-input type="text" v-model.trim="material" required placeholder="Material" />
      Número de Fixações
      <b-input type="number" v-model.trim="structure.numFixacoes" required placeholder="Número de Fixações" />
      Número de Vãos
      <b-input type="number" v-model.trim="structure.numVaos" required placeholder="Número de Vãos" />
      Sobrecarga
      <b-input type="number" v-model.trim="structure.sobrecarga" required placeholder="Sobrecarga" />
      Projeto
      <b-select v-model="projetoId" :options="projects" required value-field="id" text-field="nomeProjeto">
        <template v-slot:first>
          <option :value="null" disabled>-- Selecione o projeto --</option>
        </template>
      </b-select>
      Aplicação
      <b-select v-model="applicationId" :options="application" required value-field="name" text-field="name">
        <template v-slot:first>
          <option :value="null" disabled>-- Selecione a aplicação --</option>
        </template>
      </b-select>
      Combinação Acões Verificacao De Formacao
      <b-select v-model="combinacaoAcoesVerificacaoDeFormacaoId" :options="combinacaoAcoesVerificacaoDeFormacao" required value-field="name" text-field="name">
        <template v-slot:first>
          <option :value="null" disabled>-- Selecione a Combinação Acoes Verificacao De Formacao --</option>
        </template>
      </b-select>
      <div>
      Sobre Carga Categoria
      <b-select v-model="sobreCargaCategoriaId" :options="sobreCargaCategoria" required value-field="code" text-field="name">
        <template v-slot:first>
          <option :value="null" disabled>-- Selecione a Sobre Carga Categoria --</option>
        </template>
      </b-select>
      </div>
      Familias disponiveis:
      <b-table striped over :items="familias" :fields="fields">
        <template v-slot:cell(actions)="row">
          <button class="btn btn-link" v-on:click.prevent="addFamilias(row.item)" >Add</button>
        </template>
      </b-table>

      Familias adicionadas
      <b-table striped over :items="addedFamilias" :fields="fields">
        <template v-slot:cell(actions)="row">
          <button class="btn btn-link" v-on:click.prevent="removeFamilias(row.item)" >Remove</button>
        </template>
      </b-table>
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
      material:"",
      projects: [],
      application:[],
      familias:[],
      addedFamilias:[],
      combinacaoAcoesVerificacaoDeFormacao:[],
      sobreCargaCategoria:[],
      applicationId:null,
      combinacaoAcoesVerificacaoDeFormacaoId:null,
      sobreCargaCategoriaId:null,
      projetoId: null,
      //  name: 0,
      fields: ["name", "actions"],
      structure: {
        altitudeMaior1000: true,
        angulo: null,
        aplicacao: {
          name: "Cobertura",
        },
        cargaPermanente: null,
        coeficienteCombinacaoNeve: null,
        coeficienteCombinacaoNeveNum1: 1.0,
        coeficienteCombinacaoNeveNum2: 1.0,
        coeficienteCombinacaoNeveNum3: 1.0,
        coeficienteCombinacaoNeveVento1: 1.0,
        coeficienteCombinacaoNeveVento2: 1.0,
        coeficienteCombinacaoNeveVento3: 1.0,
        coeficienteCombinacaoSobrecarga: null,
        coeficienteCombinacaoSobrecargaNum1: 1.0,
        coeficienteCombinacaoSobrecargaNum2: 1.0,
        coeficienteCombinacaoSobrecargaNum3: 1.0,
        coeficienteCombinacaoVento: 1.0,
        combinacaoAcoesVerificacaoDeformacao: {
          name: "Característica",
        },
        comprimentoVao: null,
        contraventamentoLateral: null,
        contraventamentoTotal: true,
        contribuicaoChapaRevestimento: false,
        espacamentoEntreVigas: null,
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
        material: "",
        neve: 1.0,
        numFixacoes: null,
        numVaos: null,
        pressaoVento: 1.0,
        projeto: {
          id: 4,
        },
        sobrecarga: null,
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

      this.$axios
      .$get("http://localhost:8080/projetoEstruturas/api/application/")
      .then((application) => {
        this.application = application;
      });

      this.$axios
      .$get("http://localhost:8080/projetoEstruturas/api/combinacaoAcoesVerificacaoDeFormacao/")
      .then((combinacaoAcoesVerificacaoDeFormacao) => {
        this.combinacaoAcoesVerificacaoDeFormacao = combinacaoAcoesVerificacaoDeFormacao;
      });

      this.$axios
      .$get("http://localhost:8080/projetoEstruturas/api/sobreCargaCategoria/")
      .then((sobreCargaCategoria) => {
        this.sobreCargaCategoria = sobreCargaCategoria;
      });

      this.$axios
      .$get("http://localhost:8080/projetoEstruturas/api/family/")
      .then((familias) => {
        this.familias = familias;
      });
  },
  computed: {
    id() {
      return this.$route.params.id;
    }
  },
  methods: {
    reset() {
      this.errorMsg = false;
    },addFamilias(familia){
        this.addedFamilias.push(familia);
        this.familias = this.familias.filter(item => item.name !== familia.name)

    },
    removeFamilias(familia){
        this.addedFamilias = this.addedFamilias.filter(item => item.name !== familia.name)
        this.familias.push(familia);
    },
    create() {
      this.$axios
        .$post("http://localhost:8080/projetoEstruturas/api/structures", {
          altitudeMaior1000: true,
          angulo: this.structure.angulo,
          aplicacao: {
            name: this.applicationId,
          },
          cargaPermanente: this.structure.cargaPermanente,
          coeficienteCombinacaoNeve: this.structure.coeficienteCombinacaoNeve,
          coeficienteCombinacaoNeveNum1: 1.0,
          coeficienteCombinacaoNeveNum2: 1.0,
          coeficienteCombinacaoNeveNum3: 1.0,
          coeficienteCombinacaoNeveVento1: 1.0,
          coeficienteCombinacaoNeveVento2: 1.0,
          coeficienteCombinacaoNeveVento3: 1.0,
          coeficienteCombinacaoSobrecarga: this.structure.coeficienteCombinacaoSobrecarga,
          coeficienteCombinacaoSobrecargaNum1: 1.0,
          coeficienteCombinacaoSobrecargaNum2: 1.0,
          coeficienteCombinacaoSobrecargaNum3: 1.0,
          coeficienteCombinacaoVento: this.structure.coeficienteCombinacaoVento,
          combinacaoAcoesVerificacaoDeformacao: {
            name: this.combinacaoAcoesVerificacaoDeFormacaoId,
          },
          comprimentoVao: this.structure.comprimentoVao,
          contraventamentoLateral: this.structure.contraventamentoLateral,
          contraventamentoTotal: true,
          contribuicaoChapaRevestimento: false,
          espacamentoEntreVigas: this.structure.espacamentoEntreVigas,
          familias: this.addedFamilias,
          inerciaChapaRevestimento: "9.8e-8",
          limiteDeformacao: 1.0,
          material: this.material,
          neve: 1.0,
          numFixacoes: this.structure.numFixacoes,
          numVaos: this.structure.numVaos,
          pressaoVento: 1.0,
          projeto: {
            id: this.projetoId,
          },
          sobrecarga: this.structure.sobrecarga,
          sobrecargaCategoria: {
            code: this.sobreCargaCategoriaId,
          },
          succaoVento: 1.0,
        })
        .then((structures) => {
          this.$router.push("/projectists/"+this.id+"/structures");
        })
        .catch((error) => {
          this.errorMsg = error.response.data;
        });
    },
  },
};
</script>
