<script>
import appConfig from '@src/app.config'
import Layout from '@layouts/main'
import PageHeader from '@components/page-header'

import axios from 'axios'
export default {
  page: {
    title: 'Advanced Tables',
    meta: [{ name: 'description', content: appConfig.description }],
  },
  components: { Layout, PageHeader },
  data() {
    return {
      pdaedate: {},
      submitted: false,
      submit: false,
      subb: false,
      show: false,
      pPDA: null,
      edit: false,
      totalRows: 1,
      currentPage: 1,
      perPage: 10,
      pageOptions: [10, 25, 50, 100],
      filter: null,
      filterOn: [],
      sortBy: 'age',
      sortDesc: false,
      infoModal: {
        id: 'info-modal',
        title: '',
        content: '',
      },
      fields: [
        { key: 'snPDA', label: 'Numéro de série PDA', sortable: true },
        { key: 'designationPDA', label: 'designation PDA', sortable: true },
        { key: 'dateaffecPDA', label: 'date Affectation PDA', sortable: true },
        { key: 'mat_user', label: 'Matricule', sortable: true },
        { key: 'Suppresion', label: 'Actions' },
      ],
    }
  },
  computed: {
    /**
     * Total no. of records
     */
    rows() {
      return this.pPDA
    },

    editableFields() {
      return this.fields.filter((field) => {
        return field.editable === true
      })
    },
  },
  async mounted() {
    await this.getPda()
    // Set the initial number of items
    this.totalRows = this.pPDA
  },
  methods: {
    Test1(){
      console.log('this.pPDA.snPDA',this.pPDA.snPDA)
      console.log('this.pPDA',this.pPDA)
      console.log("update",this.pPDA)
      return;
        axios
      .patch(`https://pfeapis.herokuapp.com/ligne/${this.pPDA.snPDA}`,this.pPDA)
      .then( async (res) => {
       console.log("resUpdate",res)
        await this.getPda()
        this.show = false
        this.submitted = true
       
      })
    },
    
    async getPda() {
      axios.get(`https://pfeapis.herokuapp.com/ligne/`).then(async (res) => {
        console.log('res.data',res.data)
        this.pPDA = res.data
      })
    },
    onFiltered(filteredItems) {
      // Trigger pagination to update the number of buttons/pages due to filtering
      this.totalRows = filteredItems.length
      this.currentPage = 1
    },
    info(item, index, button) {
      this.infoModal.title = `Row index: ${index}`
      this.infoModal.content = JSON.stringify(item, null, 2)
      this.$root.$emit('bv::show::modal', this.infoModal.id, button)
    },
    resetInfoModal() {
      this.infoModal.title = ''
      this.infoModal.content = ''
    },
    Edit(pda) {
      console.log('icon edit click')
      this.show = true
      this.pPDA=pda
    },

    Delete(id) {
      this.$swal
        .fire({
          title: 'Voulez-vous supprimer ?',
          text: 'Voulez-vous Confirmer cette Supression!',
          icon: 'warning',
          showCancelButton: true,
          confirmButtonColor: '#3085d6',
          cancelButtonColor: '#d33',
          confirmButtonText: 'Oui, Supprimer!',
        })
        .then(async (result) => {
          if (result.isConfirmed) {
            axios
              .delete(`https://pfeapis.herokuapp.com/ligne/${id}`)
              .then(async (res) => {
                this.subb = true
                await this.getPda()
                console.log('delete',res)
              })
            this.$swal.fire('Supprimer!', 'PDA Supprimer.', 'succée')
          }
        })
    },
  },

   updatePDA(){
     console.log("update")
      this.pdaedate={
        "dateaffecPDA":pPDA.dateaffecPDA,
        "designationPDA":pPDA.designationPDA,
        "mat_user": pPDA.mat_user
      }
 
  },
}
</script>
<template>
  <Layout>
    <PageHeader :title="title" :items="items" />

    <div class="row">
      <div class="col-12">
        <div class="col-xl-4">
          <b-modal v-model="show">
            <!--form here -->

            <div class="card">
              <div class="card-body">
                <h4 class="header-title mt-0 mb-1 text-center"
                  >Mise à jour PDA</h4
                >
                <div class="alert alert-success" v-if="submitted">
                  PDA Mise à jourer avec Succée
                </div>
    
                <form>
                  <div class="form-group">
                    <label for="snPDA">
                      Numéro de Série PDA
                      <span class="text-danger">*</span>
                    </label>
                    <input
                      disabled
                      id="snPDA"
                      v-model="pPDA.snPDA"
                      name="snPDA"
                      class="form-control"
                      :class="{
                        'is-invalid': submitted && $v.pPDA.snPDA.$error,
                      }"
                      type="number"
                      placeholder="Enterée Numéro de Série PDA"
                    />
                    <div
                      v-if="submitted && !$v.pPDA.snPDA.required"
                      class="invalid-feedback"
                      >Invalide Numéro de Série PDA.</div
                    >
                  </div>
                  <div class="form-group">
                    <label for="designationPDA"
                      >Designiation
                      <span class="text-danger">*</span>
                    </label>
                    <textarea
                      id="designationPDA"
                      v-model="pPDA.designationPDA"
                      name="designationPDA"
                      class="form-control"
                      :class="{
                        'is-invalid':
                          submitted && $v.pPDA.designationPDA.$error,
                      }"
                      placeholder="ajoutez plusieurs lignes"
                    ></textarea>
                    <div
                      v-if="submitted && !$v.pPDA.designationPDA.required"
                      class="invalid-feedback"
                      >Invalide Designiation.</div
                    >
                  </div>
                  <div class="form-group">
                    <label for="dateaffecPDA">
                      Date affectation
                      <span class="text-danger">*</span>
                    </label>
                    <input
                      id="dateaffecPDA"
                      v-model="pPDA.dateaffecPDA"
                      name="dateaffecPDA"
                      class="form-control"
                      :class="{
                        'is-invalid':
                          submitted && $v.pPDA.dateaffecPDA.$error,
                      }"
                      type="date"
                      placeholder="Enterée Date affectation"
                    />
                    <div
                      v-if="submitted && !$v.pPDA.dateaffecPDA.required"
                      class="invalid-feedback"
                      >Invalide Date affectation.</div
                    >
                  </div>
                  <div class="form-group">
                    <label for="mat_user">
                      Matricule
                      <span class="text-danger">*</span>
                    </label>
                    <input
                      id="mat_user"
                      v-model="pPDA.mat_user"
                      name="mat_user"
                      class="form-control"
                      :class="{
                        'is-invalid': submitted && $v.pPDA.mat_user.$error,
                      }"
                      type="text"
                      placeholder="Enterée Matricule"
                    />
                    <div
                      v-if="submitted && !$v.pPDA.mat_user.required"
                      class="invalid-feedback"
                      >Invalide Matricule.</div
                    >
                  </div>
                  <b-button
                    variant="primary"
                    style="background-color: blue !important"
                     type="submit"
                    >Mise à jours</b-button
                  >
                  <button @click="Test1()">Test 1</button>
                </form>

             
              </div>
            </div>
            <!--endForm-->
             <template v-slot:modal-footer>
              <b-button variant="light" @click="show = false"
                    >Close</b-button
                  >
                   
            </template>
          </b-modal>
        </div>
      </div>
      <div class="col-12">
        <div class="card">
          <div class="card-body">
            <h4 class="header-title mt-0 mb-1"
              >Affichage et Supprision des PDA</h4
            >
            <p class="text-muted font-13 mb-4"></p>
            <div class="row mb-md-2">
              <div class="col-sm-12 col-md-6">
                <div id="tickets-table_length" class="dataTables_length">
                  <label class="d-inline-flex align-items-center">
                    Montrer&nbsp;
                    <b-form-select
                      v-model="perPage"
                      size="sm"
                      :options="pageOptions"
                    ></b-form-select
                    >&nbsp;entrées
                  </label>
                </div>
              </div>
              <!-- Search -->
              <div class="col-sm-12 col-md-6">
                <div
                  id="tickets-table_filter"
                  class="dataTables_filter text-md-right"
                >
                  <label class="d-inline-flex align-items-center">
                    Rechercher:
                    <b-form-input
                      v-model="filter"
                      type="search"
                      placeholder="Rechercher..."
                      class="form-control form-control-sm ml-2"
                    ></b-form-input>
                  </label>
                </div>
              </div>
              <!-- End search -->
            </div>
            <!-- Table -->
            <div class="table-responsive mb-0">
              <b-table
                :items="this.pPDA"
                :fields="fields"
                responsive="sm"
                :per-page="perPage"
                :current-page="currentPage"
                :sort-by.sync="sortBy"
                :sort-desc.sync="sortDesc"
                :filter="filter"
                :filter-included-fields="filterOn"
                @filtered="onFiltered"
              >
                <template #cell(dateaffecPDA)="row">
                  {{
                    String(row.item.dateaffecPDA).length > 0
                      ? String(row.item.dateaffecPDA).substr(0, 10)
                      : row.item.dateaffecPDA
                  }}
                </template>
                <template #cell(Suppresion)="row">
                  <b-button @click="Edit(row.item)">
                    <i class="uil uil-edit Btn-Action-green"></i>
                  </b-button>

                  <b-button @click="Delete(row.item._id)">
                    <i class="uil uil-trash-alt Btn-Action-red"></i>
                  </b-button>
                </template>
              </b-table>
            </div>

            <div class="row">
              <div class="col">
                <div
                  class="dataTables_paginate paging_simple_numbers float-right"
                >
                  <ul class="pagination pagination-rounded mb-0">
                    <!-- pagination -->
                    <b-pagination
                      v-model="currentPage"
                      :total-rows="rows"
                      :per-page="perPage"
                    ></b-pagination>
                  </ul>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </Layout>
</template>
<style scoped>
.Btn-Action-green {
  font-size: 20px;
  color: #43d39e;
}
.Btn-Action-red {
  font-size: 20px;
  color: #ed1c24;
}
.btn:not(:disabled):not(.disabled) {
  cursor: pointer;
  background-color: white;
  border: none;
}
</style>