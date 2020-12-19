<template>
  <div class="container-fluid">
    <h4>Lista de Estuturas</h4>
      <b-input v-model.trim="material" placeholder="Escreva o material" />

      <button @click.prevent="filter">Filtrar</button>
      <button @click.prevent="reset">Reset</button>
    <b-table striped over :items="structures" :fields="structureFields">
      <template v-slot:cell(actions)="row">
        <nuxt-link class="btn btn-link" :to="`/projectists/${id}/structures/${row.item.id}`">Ver</nuxt-link>
        <nuxt-link class="btn btn-link" :to="`/projectists/${id}/structures/${row.item.id}/update`">Update</nuxt-link>
        <button class="btn btn-link" v-on:click.prevent="apagar(row.item.id)">Eliminar</button>
      </template>
    </b-table>
    <nuxt-link class="btn btn-link" :to="`/projectists/${id}/structures/create`">Create structure</nuxt-link>
    
      <nuxt-link :to="`/projectists/${this.id}`">Voltar</nuxt-link>
  </div>
</template>
<script>
export default {
  data() {
    return {
      material: "",
      structures: [],
      structureFields: [
        "id",
        "material",
        "aplicacao.name",
        "numVao",
        "numFixacoes",
        "sobrecarga",
        "comprimentoVao",
        "actions",
      ],
    };
  },
  computed:{
    id() {
      return this.$route.params.id
    },
  },
  created() {
    this.$axios
      .$get(`http://localhost:8080/projetoEstruturas/api/structures`)
      .then((structures) => (this.structures = structures || {}));
  },
  methods: {
    apagar(id) {
      console.log("dsofddsafouihb");
      this.$axios
        .$delete(`http://localhost:8080/projetoEstruturas/api/structures/${id}`)
        .then((response) => {
            this.$axios
            .$get(`http://localhost:8080/projetoEstruturas/api/structures`)
            .then((structures) => (this.structures = structures || {}));
        });
    },filter(){
                this.$axios.$post('http://localhost:8080/projetoEstruturas/api/structures/filter',{
                    material: this.material
                    })
                .then(structures => { this.structures = structures})
            },
            reset(){
                this.$axios.$get(`http://localhost:8080/projetoEstruturas/api/structures`)
                .then(structures => this.structures = structures || {})
                this.material = ""
            }
  },
};
</script>
