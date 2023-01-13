<template>
  <div class="d-flex flex-column justify-content-center align-items-center h-100">
    <div class="title text-center font-weight-bolder justify-content-start">
      <h1>Login</h1>
    </div>
    <b-form @submit="onSubmit">
      <b-form-group id="input-group-1" label="Seu email:" label-for="input-1">
        <b-form-input
          id="input-1"
          v-model="form.email"
          placeholder="Email"
          required
        ></b-form-input>
      </b-form-group>

      <b-form-group id="input-group-2" label="Sua Senha:" label-for="input-2">
        <b-form-input
          id="input-2"
          type="password"
          v-model="form.password"
          placeholder="Senha"
          required
        ></b-form-input>
      </b-form-group>

      <div class="d-flex justify-content-center mt-3">
        <button type="submit" class="btn btn-primary">Entrar</button>
      </div>
    </b-form>
  </div>
</template>

<style scoped src="@/assets/css/login.css"></style>

<script>

export default {

  data() {
    return {
      form: {
        email:'',
        password:''
      }
    }
  },

  methods: {
    onSubmit(event) {
      event.preventDefault()
      console.log(`start login as: `, this.form);
      this.$auth.loginWith('laravelSanctum',{
        data: this.form
      }).then((response) => {
        console.log(`login success: `, response);
        this.$router.push('/add-cv');
      }).catch((error) => {
        console.log(`login error: `, error);
      });
    }
  }
}
</script>
