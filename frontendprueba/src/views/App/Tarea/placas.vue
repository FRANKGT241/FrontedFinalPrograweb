<template>
    <b-container fluid>
      <b-modal id="modal-1" ref="modal-1" size="xl" title="Agregar nuevo registro">
        <form>
          <div class="col-md-12 mb-3">
            <label for="Curso">Curso</label>
            <input
              required
              type="text"
              class="form-control"
              id="Curso"
              v-model.trim="$v.form.Curso.$model"
              :state="!$v.form.Curso.$error"
              placeholder="Ingresar curso"
            >
          </div>
          <div class="col-md-12 mb-3">
            <label for="Nota_Parcial">Nota</label>
            <input
              required
              type="text"
              class="form-control"
              id="Nota_Parcial"
              v-model.trim="$v.form.Nota_Parcial.$model"
              :state="!$v.form.Nota_Parcial.$error"
              placeholder="Ingresar nota"
            >
          </div>
          <div class="col-md-12 mb-3">
            <label for="Alumno">Nombre alumno</label>
            <input
              required
              type="text"
              class="form-control"
              id="Alumno"
              v-model.trim="$v.form.Alumno.$model"
              :state="!$v.form.Alumno.$error"
              placeholder="Ingresar alumno"
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
            <label for="Curso">Curso</label>
            <input
              required
              type="text"
              class="form-control"
              id="Curso"
              v-model.trim="$v.form.Curso.$model"
              :state="!$v.form.Curso.$error"
              placeholder="Ingresar curso"
            >
          </div>
          <div class="col-md-12 mb-3">
            <label for="Nota_Parcial">Nota</label>
            <input
              required
              type="text"
              class="form-control"
              id="Nota_Parcial"
              v-model.trim="$v.form.Nota_Parcial.$model"
              :state="!$v.form.Nota_Parcial.$error"
              placeholder="Ingresar nota"
            >
          </div>
          <div class="col-md-12 mb-3">
            <label for="Alumno">Nombre alumno</label>
            <input
              required
              type="text"
              class="form-control"
              id="Alumno"
              v-model.trim="$v.form.Alumno.$model"
              :state="!$v.form.Alumno.$error"
              placeholder="Ingresar alumno"
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
      <b-modal id="modal-5" ref="modal-5" title="Activar post">
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
                        Curso
                      </th>
                      <th>
                        Nota Parcial
                      </th >
                      <th >
                        Alumno
                      </th>
                  </thead>
                  <tbody>
                      <tr v-for="datos in datosPosts" :key="datos.id">
                          <td><template><button class="btn btn-success" v-b-modal.modal-2 @click="openModal('update',datos.id)">Editar</button></template><template><button class="btn btn-danger"  v-b-modal.modal-5 @click="openModal('delete',datos.id)">Eliminar</button></template></td>
                          <td v-text="datos.Curso"></td>
                          <td v-text="datos.Nota_Parcial"></td>
                          <td v-text="datos.Alumno"></td>
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
    name: 'Alumnos',
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
          Curso: '',
          Nota_Parcial: '',
          Alumno: ''
        },
        apiBase: laravelUrl + '/api/datos/getTodos',
        typeOptions: [],
        options: [
          { value: '1', nombre: 'Título' },
          { value: '2', nombre: 'Descripción' },
          { value: '3', nombre: 'Completado' }
        ],
        columna: { value: '1', nombre: 'Titulo' },
        fields: [
          {
            name: 'Curso',
            sortField: 'Curso',
            title: 'Titulo',
            dataClass: 'list-item-heading'
          },
          {
            name: 'Nota_Parcial',
            sortField: 'Nota_Parcial',
            title: 'Descripcion',
            dataClass: 'list-item-heading'
          },
          {
            name: 'Alumno',
            sortField: 'Alumno',
            title: 'Estado',
            dataClass: 'list-item-heading'
          }
        ]
      }
    },
    validations () {
      return {
        form: {
          Curso: { required },
          Nota_Parcial: { required },
          Alumno: { required }
        }
      }
    },
    methods: {
      eliminarRegistro (datos) {
        axios.delete(laravelUrl + '/api/datos/' + datos.id).then((response) => {
          console.log('Eliminado correctamente')
          this.getDatos()
        })
      },
      getDatos () {
        axios.get(laravelUrl + '/api/datos/getTodos').then((response) => {
          this.datosPosts = response.data
        })
      },
      postEstado (accion, datos) {
        this.form.titulo = datos.title
        this.form.state = datos.completed
        this.form.id = datos.id
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
            this.form.titulo = ''
            this.form.descripcion = ''
            this.form.state = 1
            break
          }
          case 'update': {
            const dataid = this.datosPosts.find(dato => dato.id === id)
            if (dataid) {
              this.form.id = dataid.id
              this.form.Curso = dataid.Curso
              this.form.Nota_Parcial = dataid.Nota_Parcial
              this.form.Alumno = dataid.Alumno
            }
            this.$refs['modal-2'].show()
            break
          }
          case 'delete': {
            const dataidDel = this.datosPosts.find(dato => dato.id === id)
            if (dataidDel) {
              this.form.id = dataidDel.id
              this.form.Curso = dataidDel.Curso
              this.form.Nota_Parcial = dataidDel.Nota_Parcial
              this.form.Alumno = dataidDel.Alumno
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
            this.form.Curso = ''
            this.form.Nota_Parcial = ''
            this.form.Alumno = ''
            break
          }
          case 'update': {
            this.$v.$reset()
            this.$refs['modal-2'].hide()
            this.form.id = 0
            this.form.Curso = ''
            this.form.Nota_Parcial = ''
            this.form.Alumno = ''
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
        this.form.Curso = data.Curso
        this.form.Nota_Parcial = data.Nota_Parcial
        this.form.Alumno = data.Alumno
      },
      /* Guardar */
      onSave () {
        const me = this
        axios.post(laravelUrl + '/api/datos/', {
          Curso: me.form.Curso,
          Nota_Parcial: me.form.Nota_Parcial,
          Alumno: me.form.Alumno })
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
        axios.put(laravelUrl + '/api/datos/' + this.form.id, {
          Curso: me.form.Curso,
          Nota_Parcial: me.form.Nota_Parcial,
          Alumno: me.form.Alumno
        })
          .then((response) => {
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
        axios.delete(laravelUrl + '/api/datos/' + this.form.id).then((response) => {
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
  