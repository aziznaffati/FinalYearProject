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
        des: '',
        date: '',
        mat: '',
      },

      submit: false,
      submitted: false,
    }
  },
  validations: {
    form: {
      sn: { required },
      des: { required },
      date: { required },
      mat: { required },
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
            <h4 class="header-title mt-0 mb-1 text-center">Mise à jour PDA</h4>
            <form @submit.prevent="handleSubmit">
              <div class="form-group">
                <label for="sn">
                  Numéro de Série PDA
                  <span class="text-danger">*</span>
                </label>
                <input
                  id="sn"
                  v-model="form.sn"
                  name="sn"
                  class="form-control"
                  :class="{ 'is-invalid': submitted && $v.form.sn.$error }"
                  type="number"
                  placeholder="Enterée Numéro de Série PDA"
                />
                <div
                  v-if="submitted && !$v.form.sn.required"
                  class="invalid-feedback"
                  >Invalide Numéro de Série PDA.</div
                >
              </div>
              <div class="form-group">
                <label for="des"
                  >Designiation
                  <span class="text-danger">*</span>
                </label>
                <textarea
                  id="des"
                  v-model="form.des"
                  name="des"
                  class="form-control"
                  :class="{ 'is-invalid': submitted && $v.form.des.$error }"
                  placeholder="ajoutez plusieurs lignes"
                ></textarea>
                <div
                  v-if="submitted && !$v.form.des.required"
                  class="invalid-feedback"
                  >Invalide Designiation.</div
                >
              </div>
              <div class="form-group">
                <label for="date">
                  Date affectation
                  <span class="text-danger">*</span>
                </label>
                <input
                  id="date"
                  v-model="form.date"
                  name="date"
                  class="form-control"
                  :class="{ 'is-invalid': submitted && $v.form.date.$error }"
                  type="date"
                  placeholder="Enterée Date affectation"
                />
                <div
                  v-if="submitted && !$v.form.date.required"
                  class="invalid-feedback"
                  >Invalide Date affectation.</div
                >
              </div>
              <div class="form-group">
                <label for="mat">
                  Matricule
                  <span class="text-danger">*</span>
                </label>
                <input
                  id="mat"
                  v-model="form.mat"
                  name="mat"
                  class="form-control"
                  :class="{ 'is-invalid': submitted && $v.form.mat.$error }"
                  type="text"
                  placeholder="Enterée Matricule"
                />
                <div
                  v-if="submitted && !$v.form.mat.required"
                  class="invalid-feedback"
                  >Invalide Matricule.</div
                >
              </div>
              <div class="form-group text-center m-b-0">
                <button class="btn btn-primary" type="submit">Mise à jour</button>
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
