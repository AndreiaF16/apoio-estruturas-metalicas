<template>
<div class="container-fluid">
    <b-radio-group>
        <b-form-radio v-model="veredito" name="Aceitar" value="true">Aceitar</b-form-radio>
        <b-form-radio v-model="veredito" name="Rejeitar" value="false">Rejeitar</b-form-radio>
    </b-radio-group>
    <b-textarea v-model.trim="observacoes" placeholder="Preencha uma observação se necessário" />

    <button class="btn btn-primary" v-on:click.prevent="vereditoFinal()">Confirmar</button>
    <button class="btn btn-link" v-on:click.prevent="voltar()">Voltar</button>
</div>
  
</template>
<script>
    export default {
        data() {
            return {
                structure: {},
                veredito: {},
                observacoes: ""
            }
        },
        methods: {
            vereditoFinal() {
                this.$axios.$put(`http://localhost:8080/projetoEstruturas/api/structures/${this.idStructure}/accept`,
                {
                    idProjeto: this.idStructure,
                    aceite: this.veredito,
                    observacoes: this.observacoes
                })
                .then(response => {
                    this.$router.push('/clients/'+ this.structure.projeto.clienteId + "/"+ this.structure.projeto.id + "/structures")
                });
            },
            voltar(){
                this.$router.push('/clients/'+ this.structure.projeto.clienteId + "/"+ this.structure.projeto.id + "/structures")
            }
        },
        computed: {
            id() {
                return this.$route.params.id
            },
            projectId() {
                return this.$route.params.projectId
            },
            idStructure() {
                return this.$route.params.idStructure
            },
        },
        created() {
            this.$axios.$get(`http://localhost:8080/projetoEstruturas/api/structures/${this.idStructure}`)
                .then(structure => {
                    this.structure = structure || {}
                    if(structure.aceite != undefined){
                        this.veredito = structure.aceite
                    }
                    if(structure.observacoes != undefined){
                        this.observacoes = structure.observacoes
                    }
                })
        },
    }
</script>
