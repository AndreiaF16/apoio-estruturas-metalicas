<template>
  <b-container>
    <h3>Login no Apoio de Estruturas Metálicas</h3>
    <b-form @submit.prevent="onSubmit" @reset="onReset">
      <b-form-group label="Username" description="Preencha o seu username">
        <b-input name="username" placeholder="Seu username" v-model.trim="username" required />
      </b-form-group>
      <b-form-group label="Password" description="Preencha a sua password">
        <b-input
          name="password"
          type="password"
          placeholder="A sua password"
          v-model="password"
          required
        />
      </b-form-group>
      <b-button type="reset" class="btn-warning">Limpar</b-button>
      <b-button type="Submit" class="btn-success">Entrar</b-button>
    </b-form>
  </b-container>
</template>
<script>
    export default {
        auth: 'guest',
        data() {
            return {
                username: null,
                password: null
            };
        },
        methods: {
            onSubmit() {
                let promise = this.$auth.loginWith("local", {
                    data: {
                        username: this.username,
                        password: this.password
                    }
                });
                promise.then(() => {

                    this.$axios.$get(`http://localhost:8080/projetoEstruturas/api/users/${this.username}`)
                    .then(user => {
                        this.$toast.success("Login efetuado com sucesso!");

                        if (this.$auth.user.groups.includes('Cliente')) {
                            this.$router.push('/clients/'+ user.id)
                        } else if (this.$auth.user.groups.includes('Projetista')) {
                            this.$router.push('/projects/' + user.id)
                        }else if (this.$auth.user.groups.includes('Fabricante')) {
                            this.$router.push('/projects/' + user.id)
                        }
                    })
                    
                });
                promise.catch(() => {
                    this.$toast.error(
                        "Desculpe, não é possível efetuar o login. Verifique as suas credenciais"
                    );
                });
            },
            onReset() {
                this.username = null;
                this.password = null;
            }
        }
    };
</script>