<template>
  <b-container>
    <h3>Login no Apoio de Estruturas Metálicas</h3>
    <b-form @submit.prevent="onSubmit" @reset="onReset">
      <b-form-group label="Username" description="Enter your username">
        <b-input name="username" placeholder="Your username" v-model.trim="username" required />
      </b-form-group>
      <b-form-group label="Password" description="Enter your password">
        <b-input
          name="password"
          type="password"
          placeholder="Your password"
          v-model="password"
          required
        />
      </b-form-group>
      <b-button type="reset" class="btn-warning">Reset</b-button>
      <b-button type="Submit" class="btn-success">Submit</b-button>
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
                    this.$toast.success("You are logged in!");
                    if (this.$auth.user.groups.includes('UserTipo1')) {
                        this.$router.push('/coaches/'+this.username)
                    } else if (this.$auth.user.groups.includes('USerTIpo2')) {
                        this.$router.push('/athletes/' + this.username)
                    }else if (this.$auth.user.groups.includes('USerTipo3')) {
                        this.$router.push('/partners/' + this.username)
                    }
                });
                promise.catch(() => {
                    this.$toast.error(
                        "Sorry, you cant login. Ensure your credentials are correct"
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