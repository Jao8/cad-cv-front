
<template>
  <b-card class="add-edit-card">
    <b-card-header>
      <h1 class="text-center">Adicionar Curriculum</h1>
    </b-card-header>
    <b-card-body>
      <Form />
    </b-card-body>
    <b-card-footer class="d-flex justify-content-center">
      <b-button size="sm" @click="back" class="bg-secondary">
        Voltar
      </b-button>
    </b-card-footer>
  </b-card>
</template>

<style scoped src="@/assets/css/add-edit.css"></style>

<script>

import Form from '@/components/Form.vue'

  export default {
    components: {
      Form
    },
    methods: {
      back() {
        this.$router.push('/home');
      }
    }
  }
</script>
