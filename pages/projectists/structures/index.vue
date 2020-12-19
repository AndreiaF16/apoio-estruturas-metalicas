<template>
  <div class="container-fluid">
    <h4>Lista de Estuturas</h4>
    <b-table striped over :items="structures" :fields="structureFields">
      <template v-slot:cell(actions)="row">
        <nuxt-link class="btn btn-link" :to="`projectists/structures/${row.item.id}`"
          >Ver</nuxt-link
        >
        <nuxt-link class="btn btn-link" :to="`projectists/structures/${row.item.id}`"
          >Update</nuxt-link
        >
        <button
          style="color: white; background-color: red; border: 0"
          class="btn"
          @click="delete(row.item.id)"
          >Delete</button
        >
      </template>
    </b-table>
    <nuxt-link class="btn btn-link" :to="`structures/create`">Create structure</nuxt-link>
  </div>
</template>
<script>
export default {
  data() {
    return {
      structures: {},
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
  created() {
    this.$axios
      .$get(`http://localhost:8080/projetoEstruturas/api/structures`)
      .then((structures) => (this.structures = structures || {}));
  },
  methods: {
    delete(id) {
      console.log("EHHH");
      this.$axios
        .$delete(`http://localhost:8080/projetoEstruturas/api/structures/${id}`)
        .then((response) => console.log("deleted structure with id " + id));
    },
  },
};
</script>
