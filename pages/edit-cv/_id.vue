<template>
  <Form :curriculum="cv" />
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
  },
};
</script>
