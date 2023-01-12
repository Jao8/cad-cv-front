<template>
  <div class="d-flex flex-column justify-content-center align-items-center h-100">
    <div class="title text-center font-weight-bolder justify-content-start">
      <h1>Login</h1>
    </div>
    <form>
      <div class="form-group">
        <label for="emailInput">Email</label>
        <input type="text" class="form-control" id="emailInput" placeholder="Insira seu email">
      </div>
      <div class="form-group">
        <label for="passwordInput">Senha</label>
        <input type="text" class="form-control" id="passwordInput" placeholder="Insira sua senha">
      </div>
      <div class="d-flex justify-content-center mt-3">
        <button type="button" @click.prevent="login" class="btn btn-primary">Entrar</button>
      </div>
    </form>
  </div>
</template>

<style scoped src="@/assets/css/login.css"></style>

<script>

export default {

  data() {
    return {
      form: {
        email:'gestao@evtek.com',
        password:'gestor123'
      }
    }
  },

  methods: {
    login() {
      console.log(`start login as: `, this.form);
      this.$auth.loginWith('laravelSanctum',{
        data: this.form
      }).then((response) => {
        console.log(`login success: `, response);
      }).catch((error) => {
        console.log(`login error: `, error);
      });
    }
  }
}
</script>
