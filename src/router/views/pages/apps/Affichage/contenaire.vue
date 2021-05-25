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
      Can: [],
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
        { key: 'snC', label: 'Numéro de série chariot', sortable: true },
        {
          key: 'nserie_produit',
          label: 'Numéro de série Produit',
          sortable: true,
        },
        { key: 'date_ajout', label: 'Date Ajouts', sortable: true },
        { key: 'date_mise', label: 'Date Mise à jour', sortable: true },

        { key: 'Suppresion', label: 'Actions' },
      ],
    }
  },
  computed: {
    /**
     * Total no. of records
     */
    rows() {
      return this.Can
    },

    editableFields() {
      return this.fields.filter((field) => {
        return field.editable === true
      })
    },
  },
  async mounted() {
    await this.getCan()
    // Set the initial number of items
    this.totalRows = this.Panne
  },
  methods: {
    async getCan() {
      console.log('aaaaaaaaaaaaaaaaaaa', this.Can)
      axios.get(`https://pfeapis.herokuapp.com/contenaire/`).then(async (res) => {
        this.Can = res.data
        console.log('aaaaaaaaaaaaaaaaaaa', this.Can)
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
              .delete(`https://pfeapis.herokuapp.com/contenaire/${id}`)
              .then(async (res) => {
                this.subb = true
                await this.getCan()
                console.log(res)
              })
            this.$swal.fire('Supprimer!', 'contenaire Supprimer.', 'succée')
          }
        })
    },
  },
}
</script>
<template>
  <Layout>
    <PageHeader :title="title" :items="items" />

    <div class="row">
      <div class="col-12" v-show="edit"> </div>
      <div class="col-12">
        <div class="card">
          <div class="card-body">
            <h4 class="header-title mt-0 mb-1"
              >Affichage et Supprision des Panne</h4
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
                :items="Can"
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
                <template #cell(date_ajout)="row">
                  {{
                    String(row.item.date_ajout).length > 0
                      ? String(row.item.date_ajout).substr(0, 10)
                      : row.item.date_ajout
                  }}
                </template>
				<template #cell(date_mise)="row">
                  {{
                    String(row.item.date_mise).length > 0
                      ? String(row.item.date_mise).substr(0, 10)
                      : row.item.date_mise
                  }}
                </template>
                <template #cell(Suppresion)="row">
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