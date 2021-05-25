<script>
import appConfig from '@src/app.config'
import Layout from '@layouts/main'
import PageHeader from '@components/page-header'

import { required, email, minLength, sameAs } from 'vuelidate/lib/validators'

export default {
  page: {
    title: 'Forms Validation',
    meta: [{ name: 'description', content: appConfig.description }],
  },
  components: { Layout, PageHeader },
  data() {
    return {
      title: 'Forms Validation',

      form: {
        sn: '',
        snC: '',
        
      },

      submit: false,
      submitted: false,
    }
  },
  validations: {
    form: {
      sn: { required },
      snC: { required },
      
    },
  },
  methods: {
    /**
     * Basic Form submit
     */
    handleSubmit(e) {
      this.submitted = true

      // stop here if form is invalid
      this.$v.$touch()
    },
  },
}
</script>

<template>
  <Layout>
    <div class="row">
      <div class="col-lg-6">
        <div class="card">
          <div class="card-body">
            <h4 class="header-title mt-0 mb-1 text-center">Ajout Contenaire</h4>
            <form @submit.prevent="handleSubmit">
              <div class="form-group">
                <label for="sn">
                  Numéro de Série Produit
                  <span class="text-danger">*</span>
                </label>
                <input
                  id="sn"
                  v-model="form.sn"
                  name="sn"
                  class="form-control"
                  :class="{ 'is-invalid': submitted && $v.form.sn.$error }"
                  type="number"
                  placeholder="Enterée Numéro de Série Produit"
                />
                <div
                  v-if="submitted && !$v.form.sn.required"
                  class="invalid-feedback"
                  >Invalide Numéro de Série Produit.</div
                >
              <label for="snC">
                  Numéro de Série Chariot
                  <span class="text-danger">*</span>
                </label>
                <input
                  id="snC"
                  v-model="form.snC"
                  name="snC"
                  class="form-control"
                  :class="{ 'is-invalid': submitted && $v.form.snC.$error }"
                  type="number"
                  placeholder="Enterée Numéro de Série Chariot"
                />
                <div
                  v-if="submitted && !$v.form.snC.required"
                  class="invalid-feedback"
                  >Invalide Numéro de Série Chariot.</div
                >
              </div>
              <div class="form-group text-center m-b-0">
                <button class="btn btn-primary" type="submit"
                  >submit</button
                >
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
