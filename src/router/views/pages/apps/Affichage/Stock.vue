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
      char: [],
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
        { key: 'statuChar', label: 'statu Chariot', sortable: true },
        {
          key: 'datechargementChar',
          label: 'Date chargement Chariot',
          sortable: true,
        },
        { key: 'qteProdChar', label: 'qteProdChar', sortable: true },
        {
          key: 'nserie_produit',
          label: 'Numéro de série Produit',
          sortable: true,
        },
        {
          key: 'datedechargementChar',
          label: 'Date Déchargement Chariot',
          sortable: true,
        },
      ],
    }
  },
  computed: {
    /**
     * Total no. of records
     */
    rows() {
      return this.char
    },

    editableFields() {
      return this.fields.filter((field) => {
        return field.editable === true
      })
    },
  },
  async mounted() {
    await this.getchar()
    // Set the initial number of items
    this.totalRows = this.char
  },
  methods: {
    async getchar() {
      axios.get(`https://pfeapis.herokuapp.com/chariot/`).then(async (res) => {
        this.char = res.data
        console.log('*************************************', this.char)
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
              >Affichage et Supprision de Stock</h4
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
                  
                    <label class="d-inline-flex align-items-center">
                      Chercher Par date:
                      <b-form-input
                        v-model="filter"
                        type="date"
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
                :items="char"
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
                <template #cell(datechargementChar)="row">
                  {{
                    row.item.datechargementChar.length > 0
                      ? row.item.datechargementChar.substr(0, 10)
                      : row.item.datechargementChar
                  }}
                </template>
                <template #cell(datedechargementChar)="row">
                  {{
                    String(row.item.datedechargementChar).length > 0
                      ? String(row.item.datedechargementChar).substr(0, 10)
                      : row.item.datedechargementChar
                  }}
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