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
      user:{},
      edituser:false,
      users: [],
      userMat: [],
      form: {
        matriculeUser: '',
        email: '',
        passUser: '',
        confirmPassword: '',
        roleUser: '',
     
      },
      submitted: false,
      submit: false,
      useradded:false,
      
    }
  },
  validations: {
    form: {
      matriculeUser: { required },
      email: { required, email },
      passUser: { required, minLength: minLength(6) },
      confirmPassword: { required, sameAsPassword: sameAs('passUser') },
      roleUser: { required },
    },
    
  },
  async mounted() {
    await this.getUsers()
    this.getUserbyID()

  },
  methods: {
    createUser(e) {
    this.submitted = true
      let data = {
        email: this.form.email,
        matriculeUser: this.form.matriculeUser,
        passUser: this.form.passUser,
        roleUser: this.form.roleUser,
      }
      console.log('data : ', data)
   
      if (this.checkMatUser(data.matriculeUser) == -1) {
        axios
          .post(`https://pfeapis.herokuapp.com/users/`, data)
          .then((response) => {
            console.log(response)
            this.useradded=true;
            this.refreshForm()
            this.$emit('postcreated')
          })
      }

      // stop here if form is invalid
      this.$v.$touch()
    },
    
    refreshForm() {
      this.form = {
        matriculeUser: '',
        email: '',
        passUser: '',
        confirmPassword: '',
        roleUser: '',
        
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
    checkMatUser(mat) {
      return this.userMat.indexOf(mat);
    },
    getUserbyID(){
      axios.get('https://pfeapis.herokuapp.com/users/123').
      then(res=>{
        this.edituser=true;
        console.log('userById',res.data)
        this.user=res.data
        this.form.matriculeUser=res.data.matriculeUser;
        console.log()
      
      })
    }
    
  },
  
}
</script>

<template>
  <Layout>
    <div class="row">
      <div class="col-lg-8">
        <div class="card">
          <div class="card-body">
            <h4 class="header-title mt-0 mb-1 text-center"
              >Mise à jour Utilisateur</h4
            >
            <div class="alert alert-success" v-if="submitted">
              Utilisateur Mise à jourer avec Succée
            </div>
              <div class="alert alert-danger" v-else>
             Vérfier vos coordonnées
            </div>
            <form @submit.prevent="createUser">
              <div class="form-group">
                <label>
                  Email address
                  <span class="text-danger">*</span>
                </label>
                <input
                  v-model="form.email"
                  type="email"
                  name="email"
                  class="form-control"
                  :class="{ 'is-invalid': submitted && $v.form.email.$error }"
                  placeholder="Enter email"
                />
                <div
                  v-if="submitted && $v.form.email.$error"
                  class="invalid-feedback"
                >
                  <span v-if="!$v.form.email.required">Invalide email..</span>
                  <span v-if="!$v.form.email.email">Invalide email.</span>
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
                  v-model="form.matriculeUser"
                  name="matriculeUser"
                  class="form-control"
                  :class="{
                    'is-invalid': submitted && $v.form.matriculeUser.$error,
                  }"
                  type="text"
                  placeholder="Enterée Matricule"
                />
                <div
                  v-if="submitted && !$v.form.matriculeUser.required"
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
                  v-model="form.passUser"
                  type="password"
                  name="passUser"
                  class="form-control"
                  :class="{
                    'is-invalid': submitted && $v.form.passUser.$error,
                  }"
                  placeholder="Password"
                />
                <div
                  v-if="submitted && $v.form.passUser.$error"
                  class="invalid-feedback"
                >
                  <span v-if="!$v.form.passUser.required"
                    >Invalide mot de passe.</span
                  >
                  <span v-if="!$v.form.passUser.minLength"
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
                  v-model="form.confirmPassword"
                  type="password"
                  name="confirmPassword"
                  class="form-control"
                  :class="{
                    'is-invalid': submitted && $v.form.confirmPassword.$error,
                  }"
                  placeholder="Password"
                />
                <div
                  v-if="submitted && $v.form.confirmPassword.$error"
                  class="invalid-feedback"
                >
                  <span v-if="!$v.form.confirmPassword.required"
                    >Invalide champs.</span
                  >
                  <span v-else-if="!$v.form.confirmPassword.sameAsPassword"
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
                  v-model="form.roleUser"
                  name="roleUser"
                  class="form-control"
                  :class="{
                    'is-invalid': submitted && $v.form.roleUser.$error,
                  }"
                >
                  <option>Admin</option>
                  <option>Ligne1</option>
                  <option>Ligne2</option>
                </select>
                <div
                  v-if="submitted && !$v.form.roleUser.required"
                  class="invalid-feedback"
                  >Invalide Role.</div
                >
              </div>

              <div class="form-group m-b-0 text-center">
                <button class="btn btn-primary"  type="submit">Mise à jour</button>
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