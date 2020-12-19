<template>
<div class="container-fluid">
    <p> <b>Id:</b> {{structure.id}}</p>
    <p> <b>Material:</b> {{structure.material}}</p>
    <p> <b>Número de Vãos:</b> {{structure.numVaos}}</p>
    <p> <b>Comprimento do Vão:</b> {{structure.comprimentoVao}}</p>
    <p> <b>Espaçamento Entre Vigas:</b> {{structure.espacamentoEntreVigas}}</p>
    <p> <b>Angulo:</b> {{structure.angulo}}</p>
    <p> <b>Carga Permanente:</b> {{structure.cargaPermanente}}</p>
    <p> <b>Sobrecarga:</b> {{structure.sobrecarga}}</p>
    <p> <b>Neve:</b> {{structure.neve}}</p>
    <p> <b>Altitude Maior que 1000:</b> {{structure.altitudeMaior1000}}</p>
    <p> <b>Pressão Vento:</b> {{structure.pressaoVento}}</p>
    <p> <b>Sucção Vento:</b> {{structure.succaoVento}}</p>
    <p> <b>Contraventamento Total:</b> {{structure.contraventamentoTotal}}</p>
    <p> <b>Contraventamento Lateral:</b> {{structure.contraventamentoLateral}}</p>
    <p> <b>Contribuição Chapa Revestimento:</b> {{structure.contribuicaoChapaRevestimento}}</p>
    <p> <b>Número de Fixações:</b> {{structure.numFixacoes}}</p>
    <p> <b>Inercia Chapa Revestimento</b> {{structure.inerciaChapaRevestimento}}</p>
    <p> <b>Limite de Formação:</b> {{structure.limiteDeformacao}}</p>
    <p> <b>Coeficiente Combinação Sobrecarga:</b> {{structure.coeficienteCombinacaoSobrecarga}}</p>
    <p> <b>Coeficiente Combinação Sobrecarga Número 1:</b> {{structure.coeficienteCombinacaoSobrecargaNum1}}</p>
    <p> <b>Coeficiente Combinação Sobrecarga Número 2:</b> {{structure.coeficienteCombinacaoSobrecargaNum2}}</p>
    <p> <b>Coeficiente Combinação Sobrecarga Número 3:</b> {{structure.coeficienteCombinacaoSobrecargaNum3}}</p>
    <p> <b>Coeficiente Combinação Neve:</b> {{structure.coeficienteCombinacaoNeve}}</p>
    <p> <b>Coeficiente Combinação Neve Número 1:</b> {{structure.coeficienteCombinacaoNeveNum1}}</p>
    <p> <b>Coeficiente Combinação Neve Número 2:</b> {{structure.coeficienteCombinacaoNeveNum2}}</p>
    <p> <b>Coeficiente Combinação Neve Número 3:</b> {{structure.coeficienteCombinacaoNeveNum3}}</p>
    <p> <b>Coeficiente Combinação Neve Vento:</b> {{structure.coeficienteCombinacaoVento}}</p>
    <p> <b>Coeficiente Combinação Neve Vento 1:</b> {{structure.coeficienteCombinacaoNeveVento1}}</p>
    <p> <b>Coeficiente Combinação Neve Vento 2:</b> {{structure.coeficienteCombinacaoNeveVento2}}</p>
    <p> <b>Coeficiente Combinação Neve Vento 3:</b> {{structure.coeficienteCombinacaoNeveVento3}}</p>

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
