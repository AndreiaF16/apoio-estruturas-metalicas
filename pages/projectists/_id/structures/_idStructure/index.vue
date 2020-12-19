<template>
  <div class="container-fluid">
    <h1>Ver estrutura</h1>
      <p>Ângulo:{{structure.angulo}}</p>
      <p>Carga Permanente:{{structure.cargaPermanente}}</p>
      <p>Coeficiente de Combinação de Neve:{{structure.coeficienteCombinacaoNeve}}</p>
      <p>Coeficiente de Combinação Sobrecarga: {{structure.coeficienteCombinacaoSobrecarga}}</p>
      <p>Comprimento do Vao:{{structure.comprimentoVao}}</p>
      <p>Contraventamento Lateral:{{structure.contraventamentoLateral}}</p>
      <p>Espaçamento entre vigas:{{structure.espacamentoEntreVigas}}</p>
      <p>Material: {{structure.material}}</p>
      <p>Número de Fixações: {{structure.numFixacoes}}</p>
      <p>Número de Vãos: {{structure.numVaos}}</p>
      <p>Sobrecarga: {{structure.sobrecarga}}</p>
      <p>Projeto: {{projetoId}}</p>
     <p> Aplicação: {{applicationId}}</p>
      <p>Combinação Acões Verificacao De Formacao: {{combinacaoAcoesVerificacaoDeFormacaoId}}</p>
      <p>Sobre Carga Categoria: {{sobreCargaCategoriaId}}</p>

      Familias:
      <b-table striped over :items="this.structure.familias" :fields="fields"/>
  </div>
</template>
<script>
export default {
  data() {
    return {
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
      structure: {},
      errorMsg: false,
    };
  },
  created() {
        this.$axios
          .$get("http://localhost:8080/projetoEstruturas/api/structures/"+this.idStructure)
          .then((structure) => {
            this.structure = structure;
            this.applicationId = this.structure.aplicacao.name
            this.sobreCargaCategoriaId = this.structure.sobrecargaCategoria.name
            this.combinacaoAcoesVerificacaoDeFormacaoId = this.structure.combinacaoAcoesVerificacaoDeformacao.name
            this.projetoId = this.structure.projeto.nomeProjeto
            this.familias = this.structure.familias
          });
  },
  computed: {
    id() {
      return this.$route.params.id
    },
    idStructure() {
      return this.$route.params.idStructure;
    }
  },
  methods: {
    reset() {
      this.errorMsg = false;
    }
  },
};
</script>
