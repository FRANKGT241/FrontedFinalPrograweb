<template>
  <b-container fluid>
    <b-modal id="modal-1" ref="modal-1" size="xl" title="Agregar nuevo registro">
      <form>
        <div class="col-md-12 mb-3">
          <label for="nombre">Nombre</label>
          <input
            required
            type="text"
            class="form-control"
            id="nombre"
            v-model.trim="$v.form.nombre.$model"
            :state="!$v.form.nombre.$error"
            placeholder="Ingresar nombre"
          >
        </div>
        <div class="col-md-12 mb-3">
          <label for="descripcion">Descripcion</label>
          <input
            required
            type="text"
            class="form-control"
            id="descripcion"
            v-model.trim="$v.form.descripcion.$model"
            :state="!$v.form.descripcion.$error"
            placeholder="Ingresar descripcion"
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
          <label for="nombre">Nombre</label>
          <input
            required
            type="text"
            class="form-control"
            id="nombre"
            v-model.trim="$v.form.nombre.$model"
            :state="!$v.form.nombre.$error"
            placeholder="Ingresar nombre"
          >
        </div>
        <div class="col-md-12 mb-3">
          <label for="descripcion">Descripcion</label>
          <input
            required
            type="text"
            class="form-control"
            id="descripcion"
            v-model.trim="$v.form.descripcion.$model"
            :state="!$v.form.descripcion.$error"
            placeholder="Ingresar descripcion"
          >
        </div>
        <div class="col-md-12 mb-3">
  <label for="estado">Estado</label>
  <select
    required
    class="form-control"
    id="estado"
    v-model.trim="$v.form.estado.$model"
    :state="!$v.form.estado.$error"
  >
    <option value="" disabled selected>Seleccionar estado</option>
    <option value="pendiente">Pendiente</option>
    <option value="cancelada">Cancelada</option>
    <option value="completada">Completada</option>
  </select>
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
        ¿Desea eliminar este registro?
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
                      Nombre
                    </th>
                    <th>
                      Descripcion
                    </th>
                    <th>
                      Estado
                    </th>
                </thead>
                <tbody>
                    <tr v-for="datos in datosPosts" :key="datos.id">
                        <td><template><button class="btn btn-success" v-b-modal.modal-2 @click="openModal('update',datos.id)">Editar</button></template><template><button class="btn btn-danger"  v-b-modal.modal-5 @click="openModal('delete',datos.id)">Eliminar</button></template></td>
                        <td v-text="datos.nombre"></td>
                        <td v-text="datos.descripcion"></td>
                        <td><template><button  :class="{'btn btn-warning': datos.estado === 'pendiente', 'btn btn-danger': datos.estado === 'cancelada', 'btn btn-success': datos.estado === 'completada'}" v-text="datos.estado" @click="actualizarestado()"></button></template></td>
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
        descripcion: '',
        nombre: '',
        estado: ''
      },
      apiBase: laravelUrl + '/tarea/',
      typeOptions: [],
      options: [
        { value: '1', nombre: 'Tarea' }
      ],
      columna: { value: '1', nombre: 'Titulo' },
      fields: [
        {
          name: 'tare',
          sortField: 'tarea',
          title: 'Titulo',
          dataClass: 'list-item-heading'
        }
      ]
    }
  },
  validations () {
    return {
      form: {
        descripcion: { required },
        nombre: { required },
        estado: { required }
      }
    }
  },
  methods: {
    eliminarRegistro (datos) {
      axios.delete(laravelUrl + '/tarea/' + datos.id).then((response) => {
        console.log('Eliminado correctamente')
        this.getDatos()
      })
    },
    getDatos () {
      axios.get(laravelUrl + '/tarea/').then((response) => {
        this.datosPosts = response.data
      })
    },
    postEstado (accion, datos) {
      this.form.descripcion = datos.descripcion
      this.form.nombre = datos.nombre
      this.form.estado = datos.estado

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
          this.form.descripcion = ''
          this.form.nombre = ''
          this.form.estado = ''
          break
        }
        case 'update': {
          const dataid = this.datosPosts.find(dato => dato.id === id)
          if (dataid) {
            this.form.id = dataid.id
            this.form.descripcion = dataid.descripcion
            this.form.nombre = dataid.nombre
            this.form.estado = dataid.estado
          }
          this.$refs['modal-2'].show()
          break
        }
        case 'delete': {
          const dataidDel = this.datosPosts.find(dato => dato.id === id)
          if (dataidDel) {
            this.form.id = dataidDel.id
            this.form.descripcion = dataidDel.descripcion
            this.form.nombre = dataidDel.nombre
            this.form.estado = dataidDel.estado
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
          this.form.descripcion = ''
          this.form.precioPublico = ''
          this.form.nombre = ''
          this.form.estado = ''
          this.form.Nota_Parcial = ''
          this.form.modeloA = ''
          break
        }
        case 'update': {
          this.$v.$reset()
          this.$refs['modal-2'].hide()
          this.form.id = 0
          this.form.descripcion = ''
          this.form.precioPublico = ''
          this.form.nombre = ''
          this.form.estado = ''
          this.form.Nota_Parcial = ''
          this.form.modeloA = ''
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
      this.form.descripcion = data.descripcion
      this.form.nombre = data.nombre
      this.form.estado = data.estado
    },
    /* Guardar */
    onSave () {
      const me = this
      axios.post(laravelUrl + '/tarea/', {
        descripcion: me.form.descripcion,
        precioPublico: me.form.precioPublico,
        nombre: me.form.nombre,
        estado: me.form.estado,
        modeloA: me.form.modeloA })
        .then((response) => {
          me.getDatos()
          me.closeModal('save')
        })
        .catch((error) => {
          // this.errorMessage = error.message;
          console.error('Error!', error)
        })
    },
    actualizarestado () {
      let me = this
      const estados = ['cancelada', 'completada', 'pendiente']
      const indiceEstado = estados.indexOf(me.form.estado)
      const nuevoEstado = estados[(indiceEstado + 1) % estados.length]

      axios.put(laravelUrl + '/tarea/' + this.form.id, {
        descripcion: me.form.descripcion,
        nombre: me.form.nombre,
        estado: nuevoEstado
      }).then((response) => {
        me.form.estado = nuevoEstado
        this.getDatos()
      }).catch((error) => {
        console.error('Hubo un error!', error)
      })
    },
    onState () {
      let me = this
      console.log(this.form.id)
      axios.put(laravelUrl + '/tarea/' + this.form.id, {
        descripcion: me.form.descripcion,
        precioPublico: me.form.precioPublico,
        nombre: me.form.nombre,
        estado: me.form.estado,
        modeloA: me.form.modeloA }).then((response) => {
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
      axios.delete(laravelUrl + '/tarea/' + this.form.id).then((response) => {
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
