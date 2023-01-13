<template>
  <div class="d-flex justify-content-center align-items-center">
    <b-card no-body>
      <b-form class="cv form" @submit="onSubmit" @reset="onReset" v-if="show">
        <b-tabs pills card>
          <b-tab title="Info Pessoais" active>

            <b-form-group id="input-group-1" label="Seu Nome:" label-for="input-1">
              <b-form-input
                id="input-1"
                v-model="form.name"
                placeholder="Nome"
                required
              ></b-form-input>
            </b-form-group>

            <b-form-group id="input-group-2" label="Nome da Mae:" label-for="input-2">
              <b-form-input
                id="input-2"
                v-model="form.parents.mother"
                placeholder="Nome da Mae"
                required
              ></b-form-input>
            </b-form-group>

            <b-form-group id="input-group-3" label="Nome do Pai:" label-for="input-3">
              <b-form-input
                id="input-3"
                v-model="form.parents.father"
                placeholder="Nome do Pai"
                required
              ></b-form-input>
            </b-form-group>

            <b-form-group id="input-group-4" label="Data de Nascimento:" label-for="input-4">
              <b-form-datepicker
                id="input-4"
                v-model="form.birthDate"
                placeholder="Data de Nascimento"
                required
              ></b-form-datepicker>
            </b-form-group>

            <b-form-group id="input-group-5" label="CPF:" label-for="input-5">
              <b-form-input
                id="input-5"
                v-model="form.cpf"
                placeholder="CPF"
                required
              ></b-form-input>
            </b-form-group>

            <b-form-group id="input-group-6" label="CEP:" label-for="input-6">
              <b-form-input
                id="input-6"
                v-model="form.cep"
                placeholder="CEP"
                required
              ></b-form-input>
            </b-form-group>

            <b-form-group id="input-group-7" label="Cidade:" label-for="input-7">
              <b-form-input
                id="input-7"
                v-model="form.city"
                placeholder="Cidade"
                required
              ></b-form-input>
            </b-form-group>

            <b-form-group id="input-group-8" label="Bairro:" label-for="input-8">
              <b-form-input
                id="input-8"
                v-model="form.district"
                placeholder="Bairro"
                required
              ></b-form-input>
            </b-form-group>

            <b-form-group id="input-group-9" label="Logradouro:" label-for="input-9">
              <b-form-input
                id="input-9"
                v-model="form.address"
                placeholder="Logradouro"
                required
              ></b-form-input>
            </b-form-group>

            <b-form-group id="input-group-10" label="Telefone:" label-for="input-10">
              <b-form-input
                id="input-10"
                v-model="form.phone"
                placeholder="Telefone"
                required
              ></b-form-input>
            </b-form-group>

          </b-tab>

          <b-tab title="Info Profissionais">

            <b-form-group id="input-group-11" label="Cargo:" label-for="input-11">
              <b-form-select id="input-11" v-model="form.role" :options="options"></b-form-select>
            </b-form-group>

            <b-form-group id="input-group-12" label="Experiencia:" label-for="input-12">
              <b-form-textarea
                id="input-12"
                v-model="form.xp"
                placeholder="Experiencia"
                rows="3"
                max-rows="6"
                required
              ></b-form-textarea>
            </b-form-group>

          </b-tab>
        </b-tabs>

        <div class="d-flex justify-content-center mb-4">
          <b-button class="mr-3" type="submit" variant="primary">Submit</b-button>
          <b-button type="reset" variant="danger">Reset</b-button>
        </div>


      </b-form>
    </b-card>
  </div>
</template>

<style scoped src="@/assets/css/add-edit.css"></style>


<script>
export default {

  props: {
    curriculum: {
      type: Object,
      required: false,
    },
  },
  data() {
    return {
      form: {
        name: "",
        parents: {
          mother: "",
          father: "",
        },
        birthDate: null,
        cpf: "",
        cep: "",
        city: "",
        district: "",
        address: "",
        phone: "",
        role: null,
        xp: "",
      },
      show: true,
      roles: null,
      options: null,
      cv: null
    };
  },
  mounted(){
    this.loadRoles();
    if (this.$route.params && this.$route.params.id) {
      this.loadCV();
    }
  },
  methods: {
      async loadRoles() {
        console.log(this.form);
        const r = await this.$axios.get('roles');
        this.roles = r.data.roles;

        this.options = this.roles.map((role) => {
          return { value: role.id, text: role.name };
        });
      },
      async loadCV(){
        const r = await this.$axios.get(`cv/find/${this.$route.params.id}`);
        this.cv = r.data.data;
        console.log(`cv`, this.cv);

        if(this.cv){
          this.form.name = this.cv.name
          this.form.parents.mother = this.cv.mother_name
          this.form.parents.father = this.cv.father_name
          this.form.birthDate = this.cv.birth
          this.form.cpf = this.cv.cpf
          this.form.cep = this.cv.cep
          this.form.city = this.cv.city
          this.form.district = this.cv.district
          this.form.address = this.cv.address
          this.form.phone = this.cv.phone
          this.form.role = this.cv.role_id
          this.form.xp = this.cv.experience
        }

      },
      async onSubmit(event) {
        event.preventDefault();
        console.log(this.form);
        const r = await this.$axios.post('cv/insert', this.form);
        console.log(`r: ${r}`);
      },
      onReset(event) {
        event.preventDefault()
        // Reset our form values
        this.form.name = "",
        this.form.parents.mother = "",
        this.form.parents.father = "",
        this.form.birthDate = null,
        this.form.cpf = "",
        this.form.cep = "",
        this.form.city = "",
        this.form.district = "",
        this.form.address = "",
        this.form.phone = "",
        this.form.role = "",
        this.form.xp = "",
        // Trick to reset/clear native browser form validation state
        this.show = false
        this.$nextTick(() => {
          this.show = true
        })
      }
    }

};
</script>
