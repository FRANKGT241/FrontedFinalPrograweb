<template>
  <b-container fluid>
    <b-modal id="modal-1" ref="modal-1" size="xl" title="Agregar nuevo registro">
      <form>
        <div class="col-md-12 mb-3">
          <label for="plate_number">Numero de placa</label>
          <input
            required
            type="text"
            class="form-control"
            id="plate_number"
            v-model.trim="$v.form.plate_number.$model"
            :state="!$v.form.plate_number.$error"
            placeholder="Ingresar plate_number"
          >
        </div>
        <div class="col-md-12 mb-3">
          <label for="plate_type_id">Tipo de placa</label>
          <input
            required
            type="text"
            class="form-control"
            id="plate_type_id"
            v-model.trim="$v.form.plate_type_id.$model"
            :state="!$v.form.plate_type_id.$error"
            placeholder="Ingresar nota"
          >
        </div>
        <div class="col-md-12 mb-3">
          <label for="vin">VIN</label>
          <input
            required
            type="text"
            class="form-control"
            id="vin"
            v-model.trim="$v.form.vin.$model"
            :state="!$v.form.vin.$error"
            placeholder="Ingresar vin"
          >
        </div>
        <div class="col-md-12 mb-3">
          <label for="owner_name">Dueño</label>
          <input
            required
            type="text"
            class="form-control"
            id="owner_name"
            v-model.trim="$v.form.owner_name.$model"
            :state="!$v.form.owner_name.$error"
            placeholder="Ingresar dueño"
          >
        </div>
        <div class="col-md-12 mb-3">
          <label for="chasis_number">Numero de chasis</label>
          <input
            required
            type="text"
            class="form-control"
            id="chasis_number"
            v-model.trim="$v.form.chasis_number.$model"
            :state="!$v.form.chasis_number.$error"
            placeholder="Ingresar chasis"
          >
        </div>
      </form>
      <template #modal-footer="{}">
        <b-button  variant="primary" @click="onSave()"
          >Guardar</b-button
        >
        <b-button variant="danger" @click="closeModal('save')"
          >Cancelar</b-button
        >
      </template>
    </b-modal>
    <b-modal id="modal-2" ref="modal-2" title="Editar registro">
      <form>
        <div class="col-md-12 mb-3">
          <label for="plate_number">Numero de placa</label>
          <input
            required
            type="text"
            class="form-control"
            id="plate_number"
            v-model.trim="$v.form.plate_number.$model"
            :state="!$v.form.plate_number.$error"
            placeholder="Ingresar plate_number"
          >
        </div>
        <div class="col-md-12 mb-3">
          <label for="plate_type_id">Tipo de placa</label>
          <input
            required
            type="text"
            class="form-control"
            id="plate_type_id"
            v-model.trim="$v.form.plate_type_id.$model"
            :state="!$v.form.plate_type_id.$error"
            placeholder="Ingresar nota"
          >
        </div>
        <div class="col-md-12 mb-3">
          <label for="vin">VIN</label>
          <input
            required
            type="text"
            class="form-control"
            id="vin"
            v-model.trim="$v.form.vin.$model"
            :state="!$v.form.vin.$error"
            placeholder="Ingresar vin"
          >
        </div>
        <div class="col-md-12 mb-3">
          <label for="owner_name">Dueño</label>
          <input
            required
            type="text"
            class="form-control"
            id="owner_name"
            v-model.trim="$v.form.owner_name.$model"
            :state="!$v.form.owner_name.$error"
            placeholder="Ingresar dueño"
          >
        </div>
        <div class="col-md-12 mb-3">
          <label for="chasis_number">Numero de chasis</label>
          <input
            required
            type="text"
            class="form-control"
            id="chasis_number"
            v-model.trim="$v.form.chasis_number.$model"
            :state="!$v.form.chasis_number.$error"
            placeholder="Ingresar chasis"
          >
        </div>
      </form>
      <template #modal-footer="{}">
        <b-button  variant="primary" @click="onState()"
          >Guardar</b-button
        >
        <b-button variant="danger" @click="closeModal('update')"
          >Cancelar</b-button
        >
      </template>
    </b-modal>
    <b-modal id="modal-5" ref="modal-5" title="Eliminar usuario">
      <h6 class="my-4">
        ¿Desea eliminar este usuario?
      </h6>
      <template #modal-footer="{}">
        <b-button
          type="submit"
          variant="primary"
          @click="deleteUser()"
          >Eliminar</b-button
        >
        <b-button variant="danger" @click="$bvModal.hide('modal-5')"
          >Cancelar</b-button
        >
      </template>
    </b-modal>
    <b-row>
      <b-col md="12">
        <iq-card>
            <template v-slot:headerTitle>
              <h4 class="card-title mt-3">Alumnos</h4>
              <div class="iq-search-bar mt-2">
                <div class="row">
                  <div class="col-sm">
                    <b-form action="#" class="searchbox">
                        <b-input id="search" placeholder="Buscar..." @input="(val) => searchChange(val)" />
                        <a class="search-link" href="#"><i class="ri-search-line"></i></a>
                    </b-form>
                  </div>
                  <div class="col-sm">
                    <b-dropdown
                      id="ddown1"
                      :text="columna.nombre"
                      variant="outline-dark"
                      class="mr-1 float-md-left btn-group"
                      size="xs"
                    >
                      <b-dropdown-item
                        v-for="(search, index) in options"
                        :key="index"
                        @click="changeTypeSearch(search)"
                        >{{ search.nombre }}</b-dropdown-item
                      >
                    </b-dropdown>
                  </div>
                </div>
              </div>
            </template>
            <template v-slot:headerAction>
            <b-button variant="primary"  v-b-modal.modal-1>AGREGAR NUEVO</b-button>
          </template>
          <template v-slot:body>
            <table class="table">
                <thead>
                    <th>
                      Acciones
                    </th>
                    <th>
                      Número de placa
                    </th>
                    <th>
                      Id tipo placa
                    </th>
                    <th>
                      VIN
                    </th >
                    <th >
                      Dueño
                    </th>
                    <th >
                      Numero de chasis
                    </th>

                </thead>
                <tbody>
                    <tr v-for="datos in datosPosts" :key="datos.id">
                        <td><template><button class="btn btn-success" v-b-modal.modal-2 @click="openModal('update',datos.id)">Editar</button></template><template><button class="btn btn-danger"  v-b-modal.modal-5 @click="openModal('delete',datos.id)">Eliminar</button></template></td>
                        <td v-text="datos.plate_number"></td>
                        <td v-text="datos.plate_type_id"></td>
                        <td v-text="datos.vin"></td>
                        <td v-text="datos.owner_name"></td>
                        <td v-text="datos.chasis_number"></td>
                    </tr>
                </tbody>
            </table>
          </template>
        </iq-card>
      </b-col>
    </b-row>
  </b-container>
</template>
<script>
import { xray } from '../../../config/pluginInit'
// import VuetablePagination from 'vuetable-2/src/components/VuetablePagination.vue'
import useVuelidate from '@vuelidate/core'
import { required } from '@vuelidate/validators'
import axios from 'axios'
import { apiUrl, laravelUrl } from '../../../config/constant'

export default {
  name: 'vehiculos',
  components: {
  },
  setup () {
    return { $v: useVuelidate() }
  },
  mounted () {
    xray.index()
    axios.get(apiUrl + '/type/get').then((response) => {
      this.typeOptions = response.data
    })
    this.getDatos()
    console.log('Aqui ya esta montado el componente')
  },
  beforeDestroy () {
    // vacio
    this.mensajeDespedida()
  },
  beforeMount () {
    console.log('Aqui empieza el componente')
  },
  data () {
    return {
      from: 0,
      to: 0,
      total: 0,
      perPage: 5,
      search: '',
      datosPosts: [],
      form: {
        id: 0,
        plate_number: '',
        plate_type_id: '',
        vin: '',
        owner_name: '',
        chasis_number: ''
      },
      apiBase: laravelUrl + '/vehicle/',
      typeOptions: [],
      options: [
        { value: '1', nombre: 'Chasis' }
      ],
      columna: { value: '1', nombre: 'Titulo' },
      fields: [
        {
          name: 'chasis',
          sortField: 'chasis',
          title: 'Titulo',
          dataClass: 'list-item-heading'
        }
      ]
    }
  },
  validations () {
    return {
      form: {
        plate_number: { required },
        plate_type_id: { required },
        vin: { required },
        owner_name: { required },
        chasis_number: { required }
      }
    }
  },
  methods: {
    eliminarRegistro (datos) {
      axios.delete(laravelUrl + '/vehicle/' + datos.id).then((response) => {
        console.log('Eliminado correctamente')
        this.getDatos()
      })
    },
    getDatos () {
      axios.get(laravelUrl + '/vehicle/').then((response) => {
        this.datosPosts = response.data
      })
    },
    postEstado (accion, datos) {
      this.form.plate_number = datos.plate_number
      this.form.plate_type_id = datos.plate_type_id
      this.form.vin = datos.vin
      this.form.owner_name = datos.owner_name
      this.form.chasis_number = datos.chasis_number

      if (accion === 1) {
        this.mensaje('Este post esta completo')
        this.$refs['modal-3'].show()
      } else if (accion === 2) {
        this.mensaje('Este post esta incompleto')
        this.$refs['modal-4'].show()
      }
    },
    openModal (modal, id) {
      switch (modal) {
        case 'save': {
          this.$v.$reset()
          this.form.id = 0
          this.form.plate_number = ''
          this.form.plate_type_id = ''
          this.form.vin = ''
          this.form.owner_name = ''
          this.form.chasis_number = ''
          break
        }
        case 'update': {
          const dataid = this.datosPosts.find(dato => dato.id === id)
          if (dataid) {
            this.form.id = dataid.id
            this.form.plate_number = dataid.plate_number
            this.form.plate_type_id = dataid.plate_type_id
            this.form.vin = dataid.vin
            this.form.owner_name = dataid.owner_name
            this.form.chasis_number = dataid.chasis_number
          }
          this.$refs['modal-2'].show()
          break
        }
        case 'delete': {
          const dataidDel = this.datosPosts.find(dato => dato.id === id)
          if (dataidDel) {
            this.form.id = dataidDel.id
            this.form.plate_number = dataidDel.plate_number
            this.form.plate_type_id = dataidDel.plate_type_id
            this.form.vin = dataidDel.vin
            this.form.owner_name = dataidDel.owner_name
            this.form.chasis_number = dataidDel.chasis_number
          }
          this.$refs['modal-5'].show()
          break
        }
      }
    },
    closeModal (action) {
      switch (action) {
        case 'save': {
          this.$v.$reset()
          this.$refs['modal-1'].hide()
          this.form.id = 0
          this.form.plate_number = ''
          this.form.plate_type_id = ''
          this.form.vin = ''
          this.form.owner_name = ''
          this.form.Nota_Parcial = ''
          this.form.chasis_number = ''
          break
        }
        case 'update': {
          this.$v.$reset()
          this.$refs['modal-2'].hide()
          this.form.id = 0
          this.form.plate_number = ''
          this.form.plate_type_id = ''
          this.form.vin = ''
          this.form.owner_name = ''
          this.form.Nota_Parcial = ''
          this.form.chasis_number = ''
          break
        }
      }
    },
    onValidate (action) {
      this.$v.$touch()
      if (this.$v.$error !== true) {
        if (action === 'save') {
          this.onSave()
        } else if (action === 'update') {
          this.onState()
        }
      }
    },
    setData (data) {
      this.form.plate_number = data.plate_number
      this.form.plate_type_id = data.plate_type_id
      this.form.vin = data.vin
      this.form.owner_name = data.owner_name
      this.form.chasis_number = data.chasis_number
    },
    /* Guardar */
    onSave () {
      const me = this
      axios.post(laravelUrl + '/vehicle/', {
        plate_number: me.form.plate_number,
        plate_type_id: me.form.plate_type_id,
        vin: me.form.vin,
        owner_name: me.form.owner_name,
        chasis_number: me.form.chasis_number })
        .then((response) => {
          me.getDatos()
          me.closeModal('save')
        })
        .catch((error) => {
          // this.errorMessage = error.message;
          console.error('Error!', error)
        })
    },
    onState () {
      let me = this
      console.log(this.form.id)
      axios.put(laravelUrl + '/vehicle/' + this.form.id, {
        plate_number: me.form.plate_number,
        plate_type_id: me.form.plate_type_id,
        vin: me.form.vin,
        owner_name: me.form.owner_name,
        chasis_number: me.form.chasis_number }).then((response) => {
        me.$refs['modal-2'].hide()
        this.getDatos()
      })
        .catch((error) => {
          console.error('There was an error!', error)
        })
    },
    deleteUser () {
      let me = this
      console.log(this.form.id)
      axios.delete(laravelUrl + '/vehicle/' + this.form.id).then((response) => {
        me.$refs['modal-5'].hide()
        this.getDatos()
      })
        .catch((error) => {
          console.error('There was an error!', error)
        })
    },
    makeQueryParams (sortOrder, currentPage, perPage) {
      return sortOrder[0]
        ? {
          criterio: sortOrder[0] ? sortOrder[0].sortField : 'createdAt',
          order: sortOrder[0] ? sortOrder[0].direction : 'desc',
          page: currentPage,
          limit: this.perPage,
          search: this.search,
          columna: this.columna.value
        }
        : {
          criterio: sortOrder[0] ? sortOrder[0].sortField : 'createdAt',
          order: sortOrder[0] ? sortOrder[0].direction : 'desc',
          page: currentPage,
          limit: this.perPage,
          search: this.search,
          columna: this.columna.value
        }
    },
    changePageSizes (perPage) {
      this.perPage = perPage
      this.$refs.vuetable.refresh()
    },
    searchChange (val) {
      this.search = val.toLowerCase()
      this.$refs.vuetable.refresh()
    },
    onPaginationData (paginationData) {
      this.from = paginationData.from
      this.to = paginationData.to
      this.total = paginationData.total
      this.lastPage = paginationData.last_page
      this.items = paginationData.data
      this.$refs.pagination.setPaginationData(paginationData)
    },
    onChangePage (page) {
      this.$refs.vuetable.changePage(page)
    },
    changeTypeSearch (columna) {
      this.columna = columna
    },
    mensaje (mensaje) {
      this.form.mensaje = mensaje
    },
    mensajeDespedida () {
      console.log('Antes de irnos del componente manda este mensaje')
    }
  }
}
</script>
