<template>
    <div class="jumbotron">
      <h2>Formulario</h2>
      <hr />
      <hr />
  
      <vue-form :state="formState" @submit.prevent="enviar()">
  
        <validate tag="div">
          <label for="nombre">Nombre</label>
          <input
            type="text"
            id="nombre"
            class="form-control"
            autocomplete="off"
            v-model.trim="formData.nombre"
            name="nombre"
            required
            :minlength="nombreMinLength"
            nombre-max-length
            no-espacios
          />
  
          <field-messages name="nombre" show="$dirty">
            <div slot="required" class="alert alert-danger mt-1">
              Campo necesario
            </div>
            <div slot="minlength" class="alert alert-danger mt-1">
              Este campo debe poseer al menos {{ nombreMinLength }} caracteres.
            </div>
            <div slot="nombre-max-length" class="alert alert-danger mt-1">
              Este campo debe poseer máximo {{ nombreMaxLength }} caracteres.
            </div>
            <div slot="no-espacios" class="alert alert-danger mt-1">
              Solo colocar nombre sin espacios "no-espacios"
            </div>
          </field-messages>
        </validate>
  
        <br />
  
        <validate tag="div">
          <label for="descripcion">descripcion</label>
          <input
            type="text"
            id="descripcion"
            class="form-control"
            autocomplete="off"
            v-model.number="formData.descripcion"
            name="descripcion"
            required

          />
          
          <field-messages name="descripcion" show="$dirty">
            <div slot="required" class="alert alert-danger mt-1">
              Campo requerido
            </div>


          </field-messages>
        </validate>
  
        <br />
  
        <validate tag="div">
          <label for="precio">precio</label>
          <input
            type="number"
            id="precio"
            class="form-control"
            autocomplete="off"
            v-model.trim="formData.precio"
            name="precio"
            required
          />
          
          <field-messages name="precio" show="$dirty">
            <div slot="required" class="alert alert-danger mt-1">
              Campo requerido
            </div>

          </field-messages>
        </validate>
        <br />
  
        <button class="btn btn-outline-dark my-3" :disabled="formState.$invalid">
          Enviar
        </button>
      </vue-form>
  

      <label for="presupuesto">presupuesto</label>
      <input
        type="number"
        id="presupuesto"
        class="form-control"
        autocomplete="off"
        v-model.trim="presupuesto"
        name="presupuesto"
        required
      />

      <div v-if="gastos.length" class="table-responsive">
          <table class="table table-dark">
            <tr>
              <th>Nombre</th>
              <th>descripcion</th>
              <th>precio</th>
              <th>Fecha</th>
            </tr>
            <tr v-for="(gasto,index) in gastos" :key="index">
              <td>{{ gasto.nombre }}</td>
              <td>{{ gasto.descripcion }}</td>
              <td>${{ gasto.precio }}</td>
              <td>{{ gasto.fecha }}</td>
            </tr>
            <!-- <tr :class="colorTotal">Gasto total ${{gastosTotal}}</tr> -->
            <tr :style="{color: colorTotal}">Gasto total ${{gastosTotal}}</tr>
          </table>
        </div>
        <h4 v-else class="alert alert-danger text-center">Acá abajo podés ver los gastos realizados</h4>
  
    </div>
  </template>
  
  <script>
  export default {
    name: "src-componentes-formulario",
    data () {
        return {
          formState : {},
          formData: this.getInitialData(),
          nombreMinLength: 3,
          nombreMaxLength: 15,
          edadMin: 18,
          edadMax: 120,
          gastos: [],
          montoVerde: 1000,
          montoMagenta: 5000,
          presupuesto: undefined
        }
      },
    computed: {
      gastosTotal() {
        let total = this.gastos.reduce( (acc, item) => acc += parseInt(item.precio), 0);
        return total;
      },
      colorTotal() {
        let color;
        let total = this.gastosTotal;
        if (total < this.montoVerde) {
          color = "green";
        } else if (total < this.montoMagenta) {
          color = "magenta";
        } else {
          color = "orange";
        }

        if (![undefined, null, '', 0].includes(total) && total > this.presupuesto) 
          color = "red";

        return color;
      }
    },
    methods: {
        getInitialData() {
          return {
            nombre: null,
            descripcion: null,
            precio: null,
            fecha: null
          }
      },

      getFecha() {
        let fecha = new Date();

        let days = (fecha.getDate() < 10 ? '0' : '') + fecha.getDate();
        let month = (fecha.getMonth() < 9 ? '0' : '') + (fecha.getMonth() + 1);
        let year = fecha.getFullYear();
        let hour = (fecha.getHours() < 9 ? '0' : '') + fecha.getHours();
        let minutes = (fecha.getMinutes() < 9 ? '0' : '') + fecha.getMinutes();
        let seconds = (fecha.getSeconds() < 9 ? '0' : '') + fecha.getSeconds();

        return `${days}/${month}/${year} ${hour}:${minutes}:${seconds}`;
      },
      enviar() {
        let gasto = {...this.formData, fecha: this.getFecha()}
        this.gastos.push(gasto)
        this.formData = this.getInitialData()
        this.formState._reset()
      }
      }
  };
  </script>
  
  <style scoped>
  </style>