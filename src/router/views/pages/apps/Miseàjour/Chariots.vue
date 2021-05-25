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
        snC: '',
        stat: '',
        date: '',
        date2: '',
        qte: '',
        sn: '',
      },

      submit: false,
      submitted: false,
    }
  },
  validations: {
    form: {
      snC: { required },
      stat: { required },
      date: { required },
      date2: { required },
      qte: { required },
      sn: { required },
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
            <h4 class="header-title mt-0 mb-1 text-center">Mise à jour Chariot</h4>
            <form @submit.prevent="handleSubmit">
              <div class="form-group">
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
              <div class="form-group">
                <label for="stat"
                  >Satut Chariot
                  <span class="text-danger">*</span>
                </label>
                <select
                  id="stat"
                  v-model="form.stat"
                  name="stat"
                  class="form-control"
                  :class="{ 'is-invalid': submitted && $v.form.stat.$error }"
                >
                  <option>Chariot Eonvoyée</option>
                  <option>Chariot Réceptioner</option>
                </select>
                <div
                  v-if="submitted && !$v.form.stat.required"
                  class="invalid-feedback"
                  >Invalide Satut Chariot.</div
                >
              </div>
              <div class="form-group">
                <label for="date">
                  Date Chargment
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
                <label for="date2">
                  Date Déchargment
                  <span class="text-danger">*</span>
                </label>
                <input
                  id="date2"
                  v-model="form.date2"
                  name="date2"
                  class="form-control"
                  :class="{ 'is-invalid': submitted && $v.form.date2.$error }"
                  type="date"
                  placeholder="Enterée Date Déchargment"
                />
                <div
                  v-if="submitted && !$v.form.date2.required"
                  class="invalid-feedback"
                  >Invalide Date Déchargment.</div
                >
              </div>
              <div class="form-group">
                <label for="qte">
                  Quantité
                  <span class="text-danger">*</span>
                </label>
                <input
                  id="qte"
                  v-model="form.qte"
                  name="qte"
                  class="form-control"
                  :class="{ 'is-invalid': submitted && $v.form.qte.$error }"
                  type="number"
                  placeholder="Enterée Quantité"
                />
                <div
                  v-if="submitted && !$v.form.qte.required"
                  class="invalid-feedback"
                  >Invalide Quantité.</div
                >
              </div>
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
              </div>
              <div class="form-group text-center m-b-0">
                <button class="btn btn-primary" type="submit"
                  >Mise à jour</button
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
