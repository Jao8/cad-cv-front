<template>
  <div>
    <b-card class="text-center">
      <b-card-header>
        <h1>Lista de Curriculos</h1>
      </b-card-header>
      <div>
        <b-table striped hover
          :items="items" :fields="fields"
          :current-page="currentPage"
          :per-page="perPage"
          :filter="filter"
          :filter-included-fields="filterOn"
          :sort-by.sync="sortBy"
          :sort-desc.sync="sortDesc"
          :sort-direction="sortDirection"
        >
          <template #cell(actions)="{ item }">
            <b-button size="sm" @click="edit(item)" class="bg-success mr-1">
              Editar
            </b-button>
            <b-button size="sm" @click="curriculumId = item.id" class="bg-danger" v-b-modal.modal-remove>
              Remover
            </b-button>
          </template>
          <template #cell(status)="{ item }">
            <div v-if="verifyUser(item)">
              <p>{{ item.status }}</p>
              <b-button size="sm" @click="curriculumId = item.id" class="bg-primary mr-1" v-b-modal.modal-status>
                Alterar Status
              </b-button>
            </div>
          </template>
        </b-table>
      </div>
      <b-card-footer>
        <b-button size="sm" @click="add" class="bg-primary">
          Adicionar Curriculum
        </b-button>
      </b-card-footer>
    </b-card>

    <b-modal id="modal-remove" title="Remove Curriculum" hide-footer>
      <div class="d-flex flex-column justify-content-around align-items-center">
        <p class="my-4">Realmente deseja excluir esse Curriculum?</p>
        <div class="flex-row">
          <b-button variant="danger" @click="remove">Sim</b-button>
          <b-button variant="secondary" @click="closeModal">Nao</b-button>
        </div>
      </div>
    </b-modal>

    <b-modal id="modal-status" title="Mudar Status" hide-footer>
      <div class="d-flex flex-column justify-content-around align-items-center">
        <p class="my-4">Qual status deseja atribuir?</p>
        <div class="flex-row">
          <b-button variant="success" @click="changeStatus('approve')">Aprovar</b-button>
          <b-button variant="danger" @click="changeStatus('reject')">Rejeitar</b-button>
          <b-button variant="secondary" @click="closeModal">Cancelar</b-button>
        </div>
      </div>
    </b-modal>
  </div>
</template>

<script>
  export default {
    data() {
      return {
        fields: [
          {
            key: 'id',
            sortable: true
          },
          {
            key: 'candidate',
            label: 'Candidate',
            sortable: true
          },
          {
            key: 'role',
            label: 'Cargo',
            sortable: true,
          },
          {
            key: 'status',
            label: 'Status',
            sortable: false,
          },
          {
            key: 'actions',
            label: 'Acoes',
            sortable: false,
          }
        ],
        items: [],
        total: null,
        currentPage: 1,
        perPage: 5,
        pageOptions: [5, 10, 15, { value: 100, text: "Show a lot" }],
        sortBy: '',
        sortDesc: false,
        sortDirection: 'asc',
        filter: null,
        filterOn: [],
        curriculumId: null,
      }
    },
    mounted(){ this.loadCvs(); },
    computed: {
      sortOptions() {
        // Create an options list from our fields
        return this.fields
          .filter(f => f.sortable)
          .map(f => {
            return { text: f.label, value: f.key }
          })
      }
    },
    methods: {
      async loadCvs() {
        const r = await this.$axios.get('cv/list');
        const cvs = r.data.data;

        this.items = cvs.map((cv) => {
          return {
            id: cv.id,
            candidate: cv.name,
            role: cv.role.name,
            status: cv.status ? cv.status.name : 'Em analise',
            actions: [],
          };
        });

        this.total = this.items.length;
      },
      closeModal(){
        this.$bvModal.hide('modal-remove');
        this.$bvModal.hide('modal-status');
      },
      async remove() {
        const r = await this.$axios.delete(`cv/delete/${this.curriculumId}`);
        console.log(`r: `, r);
        this.closeModal();
        if(r.data.status === 200) {
          this.loadCvs();
        }
      },
      verifyUser(item) { console.log(item, 'item');return this.$auth.user.type_id === 1 },
      async changeStatus(action){
        const r = await this.$axios.post(`cv/action`, {
          curriculum_id: this.curriculumId,
          action,
        });
        console.log(`r: `, r);
        this.closeModal();
        if(r.data.status === 200) {
          this.loadCvs();
        }
      },
      edit(item) {
        this.$router.push({ name: 'edit-cv-id', params: { id: item.id } });
      },
      add(){
        this.$router.push({ name: 'add-cv' });
      },
    },
  };
</script>
