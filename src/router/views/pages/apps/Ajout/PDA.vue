<script>
import appConfig from '@src/app.config'
import Layout from '@layouts/main'
import PageHeader from '@components/page-header'
import axios from 'axios'

import { required, email, minLength, sameAs } from 'vuelidate/lib/validators'

export default {
  page: {
    title: 'Forms Validation',
    meta: [{ name: 'description', content: appConfig.description }],
  },
  components: { Layout, PageHeader },
  data() {
    return {
      users: [],
      userMat: [],
      PDA: [],
      snPDA: [],
      title: 'Forms Validation',

      form: {
        snPDA: '',
        designationPDA: '',
        dateaffecPDA: '',
        mat_user: '',
      },
       test:false,
      submit: false,
      submitted:false,
      pdaadded:false,
    }
  },
  validations: {
    form: {
      snPDA: { required },
      designationPDA: { required },
      dateaffecPDA: { required },
      mat_user: { required },
    },
  },
  async mounted() {
    await this.getPDA()
    await this.getUsers()

  },
 methods: {
    async createPDA(e) {
      console.log('******** entred')
      this.submitted=true;
      console.log('this.submitted',this.submitted)
     let data = {
        snPDA: this.form.snPDA,
        designationPDA: this.form.designationPDA,
        dateaffecPDA: this.form.dateaffecPDA,
        mat_user: this.form.mat_user,
      }
      console.log('data : ', data)
   console.log('ooooooooooooo : ', this.checkMatUser(data.mat_user))
    console.log('pppppppppp : ', this.checksnPDA(data.snPDA))
    if ((this.checkMatUser(data.mat_user) > -1) && (this.checksnPDA(data.snPDA)==-1) ) {
        axios
          .post(`https://pfeapis.herokuapp.com/ligne/`, data)
          .then((response) => {
            console.log(response)
            this.pdaadded=true;
            this.refreshForm()
            this.$emit('postcreated')
          })
      }

      // stop here if form is invalid
      this.$v.$touch()
    },
    
    refreshForm() {
      this.form = {
        snPDA: '',
        designationPDA: '',
        dateaffecPDA: '',
        mat_user: '',
        submitted: false,
      }
      
    },
    async getUsers() {
      axios.get(`https://pfeapis.herokuapp.com/users/`).then(async(res) => {
        console.log(res.data)
        this.users = res.data
       await Object.keys(res.data).forEach((e) => {
      this.userMat.push(this.users[e].matriculeUser)
      console.log(' this.userMat', this.userMat)
       })
      
      })
      
    },
    checkMatUser(mat_user) {
     console.log(this.userMat.indexOf(mat_user))
     return this.userMat.indexOf(mat_user);
      
    },
    async getPDA() {
      axios.get(`https://pfeapis.herokuapp.com/ligne/`).then(async(res) => {
        console.log(res.data)
        this.PDA = res.data
       await Object.keys(res.data).forEach((e) => {
      this.snPDA.push(this.PDA[e].snPDA)
      console.log(' this.snPDA', this.snPDA)
       })
      
      })
      
    },
    checksnPDA(snPDA) {
      return this.snPDA.indexOf(snPDA);
    },
    
  },
}
</script>

<template>
  <Layout>
    <div class="row">
      <div class="col-lg-8">
        <div class="card">
          <div class="card-body">
            <h4 class="header-title mt-0 mb-1 text-center">Ajout PDA {{submitted}} </h4>
            <div class="alert alert-success" v-if="pdaadded">
              PDA Ajouter avec Succée
            </div>
              <div class="alert alert-danger" v-else>
             Vérfier vos coordonnées
            </div>
            <form @submit.prevent="createPDA">
              <div class="form-group">
                <label for="snPDA">
                  Numéro de Série PDA 
                  <span class="text-danger">*</span>
                </label>
                <input
                  id="snPDA"
                  v-model="form.snPDA"
                  name="snPDA"
                  class="form-control"
                  :class="{ 'is-invalid': submitted && $v.form.snPDA.$error }"
                  type="number"
                  placeholder="Enterée Numéro de Série PDA"
                />
                <div
                  v-if="submitted && !$v.form.snPDA.required"
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
                  v-model="form.designationPDA"
                  name="designationPDA"
                  class="form-control"
                  :class="{ 'is-invalid': submitted && $v.form.designationPDA.$error }"
                  placeholder="ajoutez plusieurs lignes"
                ></textarea>
                <div
                  v-if="submitted && !$v.form.designationPDA.required"
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
                  v-model="form.dateaffecPDA"
                  name="dateaffecPDA"
                  class="form-control"
                  :class="{ 'is-invalid': submitted && $v.form.dateaffecPDA.$error }"
                  type="date"
                  placeholder="Enterée Date affectation"
                />
                <div
                  v-if="submitted && !$v.form.dateaffecPDA.required"
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
                  v-model="form.mat_user"
                  name="mat_user"
                  class="form-control"
                  :class="{ 'is-invalid': submitted && $v.form.mat_user.$error }"
                  type="text"
                  placeholder="Enterée Matricule"
                />
                <div
                  v-if="submitted && !$v.form.mat_user.required"
                  class="invalid-feedback"
                  >Invalide Matricule.</div
                >
              </div>
              <div class="form-group text-center m-b-0">
                <button class="btn btn-primary" type="submit">Ajouter</button>
                <button type="reset" class="btn btn-secondary m-l-5 ml-1"
                  >Annuler</button
                >
              </div>
            </form>
          </div>
        </div>
        <!-- end card--->
      </div>
      <!-- end col -->
    </div>
  </Layout>
</template>
<style>
.col-lg-8 {
  margin-left: 150px;
}
</style>
