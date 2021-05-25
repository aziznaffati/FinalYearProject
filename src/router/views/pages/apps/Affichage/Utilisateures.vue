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
  components: { Layout, PageHeader,  },
  data() {
    return {
      useredit:{},
       submitted: false,
       subb:false,
      	show: false,
      users: [],
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
        { key: 'email', label: 'Email', sortable: true },
        { key: 'matriculeUser', label: 'Matricule', sortable: true },
        { key: 'roleUser', label: 'Role', sortable: true },
        { key: 'Suppresion', label: 'Actions' },
      ],
    }
  },

  computed: {
    /**
     * Total no. of records
     */
    rows() {
      return this.users
    },

    editableFields() {
      return this.fields.filter((field) => {
        return field.editable === true
      })
    },
  },
  async mounted() {
    await this.getUsers()
  
    // Set the initial number of items
    this.totalRows = this.users
  },
  methods: {
    async getUsers() {
      axios.get(`https://pfeapis.herokuapp.com/users/`).then(async (res) => {
        this.users = res.data
      })
    },

    /**
     * Search the table data with search input
     */
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
    Edit(user) {
      this.show=true;
      this.useredit=user;
      console.log(user)
    },
 
      Delete(id) {
      this.$swal
        .fire({
          title: 'Voulez-vous supprimer ?',
          text: "Voulez-vous Confirmer cette Supression!",
          icon: 'warning',
          showCancelButton: true,
          confirmButtonColor: '#3085d6',
          cancelButtonColor: '#d33',
          confirmButtonText: 'Oui, Supprimer!',
        })
        .then(async (result) => {
          if (result.isConfirmed) {
           axios
              .delete(`https://pfeapis.herokuapp.com/users/${id}`)
              .then(async(res) => {
                this.subb=true;
                await this.getUsers()
                console.log(res)
                
              })
            this.$swal.fire('Supprimer!', 'Utilisateur Supprimer.', 'succée')
          }
          })
      
        
      

    
    },
  },
  updateUser(){
    axios.get(`https://pfeapis.herokuapp.com/users/${useredit.matriculeUser}`).then(async (res) => {
        this.users = res.data
      this.submitted = true
      })

  }
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
                  >Mise à jour Utilisateur</h4
                >
                <div class="alert alert-success" v-if="subb">
                  Utilisateur Mise à jourer avec Succée
                </div>
                <div class="alert alert-danger" v-else>
                  Vérfier vos coordonnées
                </div>
                <form @submit.prevent="updateUser">
                  <div class="form-group">
                    <label>
                      Email address
                      <span class="text-danger">*</span>
                    </label>
                    <input
                      v-model="useredit.email"
                      type="email"
                      name="email"
                      class="form-control"
                      :class="{
                        'is-invalid': submitted && $v.useredit.email.$error,
                      }"
                      placeholder="Enter email"
                    />
                    <div
                      v-if="submitted && $v.useredit.email.$error"
                      class="invalid-feedback"
                    >
                      <span v-if="!$v.useredit.email.required"
                        >Invalide email..</span
                      >
                      <span v-if="!$v.useredit.email.email"
                        >Invalide email.</span
                      >
                    </div>
                  </div>
                  <div class="form-group">
                    <label for="matriculeUser">
                      Matricule
                      <span class="text-danger">*</span>
                    </label>
                    <input
                      disabled
                      id="matriculeUser"
                      v-model="useredit.matriculeUser"
                      name="matriculeUser"
                      class="form-control"
                      :class="{
                        'is-invalid':
                          submitted && $v.useredit.matriculeUser.$error,
                      }"
                      type="text"
                      placeholder="Enterée Matricule"
                    />
                    <div
                      v-if="submitted && !$v.useredit.matriculeUser.required"
                      class="invalid-feedback"
                      >Invalide Matricule.</div
                    >
                  </div>

                  <div class="form-group">
                    <label>
                      Mot de passe
                      <span class="text-danger">*</span>
                    </label>
                    <input
                      v-model="useredit.passUser"
                      type="password"
                      name="passUser"
                      class="form-control"
                      :class="{
                        'is-invalid': submitted && $v.useredit.passUser.$error,
                      }"
                      placeholder="Password"
                    />
                    <div
                      v-if="submitted && $v.useredit.passUser.$error"
                      class="invalid-feedback"
                    >
                      <span v-if="!$v.useredit.passUser.required"
                        >Invalide mot de passe.</span
                      >
                      <span v-if="!$v.useredit.passUser.minLength"
                        >Password must be at least 6 characters.</span
                      >
                    </div>
                  </div>
                  <div class="form-group">
                    <label for="passWord2">
                      Confirmée mot de passe
                      <span class="text-danger">*</span>
                    </label>
                    <input
                      id="passWord2"
                      v-model="useredit.confirmPassword"
                      type="password"
                      name="confirmPassword"
                      class="form-control"
                      :class="{
                        'is-invalid':
                          submitted && $v.useredit.confirmPassword.$error,
                      }"
                      placeholder="Password"
                    />
                    <div
                      v-if="submitted && $v.useredit.confirmPassword.$error"
                      class="invalid-feedback"
                    >
                      <span v-if="!$v.useredit.confirmPassword.required"
                        >Invalide champs.</span
                      >
                      <span
                        v-else-if="!$v.useredit.confirmPassword.sameAsPassword"
                        >Mot de passe non compatible.</span
                      >
                    </div>
                  </div>
                  <div class="form-group">
                    <label for="roleUser"
                      >Role
                      <span class="text-danger">*</span>
                    </label>
                    <select
                      id="roleUser"
                      v-model="useredit.roleUser"
                      name="roleUser"
                      class="form-control"
                      :class="{
                        'is-invalid': submitted && $v.useredit.roleUser.$error,
                      }"
                    >
                      <option>Admin</option>
                      <option>Ligne1</option>
                      <option>Ligne2</option>
                      <option>Chef de Ligne</option>
                    </select>
                    <div
                      v-if="submitted && !$v.useredit.roleUser.required"
                      class="invalid-feedback"
                      >Invalide Role.</div
                    >
                  </div>
                </form>
              </div>
            </div>
            <!--endForm-->
            <template v-slot:modal-footer>
              <b-button variant="light" @click="show = false">Close</b-button>
              <b-button
                variant="primary"
                style="background-color: blue !important"
                type="submit"
                >Mise à jours</b-button
              >
            </template>
          </b-modal>
        </div>
      </div>
      <div class="col-12">
        <div class="card">
          <div class="card-body">
            <h4 class="header-title mt-0 mb-1"
              >Affichage et Supprision des Utilisateurs</h4
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
                :items="users"
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