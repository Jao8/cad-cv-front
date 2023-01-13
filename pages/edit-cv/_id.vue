<template>
  <b-card class="add-edit-card">
    <b-card-header>
      <h1 class="text-center">Editar Curriculum</h1>
    </b-card-header>
    <b-card-body>
      <Form :curriculum="cv" />
    </b-card-body>
    <b-card-footer class="d-flex justify-content-center">
      <b-button size="sm" @click="back" class="bg-secondary"> Voltar </b-button>
    </b-card-footer>
  </b-card>
</template>

<script>
import Form from "@/components/Form.vue";

export default {
  data() {
    return { cv: null };
  },
  components: {
    Form,
  },
  mounted() {
    this.loadCV(this.$route.params.id);
  },
  methods: {
    async loadCV(id) {
      const r = await this.$axios.get(`cv/find/${id}`);
      this.cv = r.data.data;
      console.log(`resp data`, r.data.data);
    },
    back() {
      this.$router.push("/home");
    },
  },
};
</script>
