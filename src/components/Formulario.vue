<template>

  <section class="src-components-formulario">
    <h1>Formulario</h1>

    <vue-form :state="formState" @submit.prevent="enviar()">
      <validate tag="div">
        <!-- elemento de entrada -->
        <label for="nombre">Nombre</label>
        <input
          type="text" 
          id="nombre"
          name="nombre"
          class="form-control"
          autocomplete="off"
          v-model.trim="formData.nombre" 
          required
          :minlength="nombreMinLength"
          :maxlength="nombreMaxLength"
        />
  
        <!-- mensajes de validacion -->
        <field-messages name="nombre" show="$dirty">
          <!-- <div class="alert alert-success mt-1">Success!</div> -->
          <div slot="required" class="alert alert-danger mt-1">Este campo es obligatorio</div>
          <div slot="minlength" class="alert alert-danger mt-1">Este campo requiere como minimo {{nombreMinLength}} caracteres</div>
          <div slot="maxlength" class="alert alert-danger mt-1">Este campo requiere como maximo {{nombreMaxLength}} caracteres</div>
          <div slot="no-espacios" class="alert alert-danger mt-1">No se permiten espacios intermedios</div>

        </field-messages>
      </validate>

      <validate tag="div">
        <!-- elemento de entrada -->
        <label for="edad">Edad</label>
        <input
          type="number" 
          id="edad"
          name="edad"
          class="form-control"
          autocomplete="off"
          v-model.number="formData.edad" 
          required
          :min="edadMin"
          :max="edadMax"
        />
  
        <field-messages name="edad" show="$dirty">
          <div slot="required" class="alert alert-danger mt-1">Este campo es obligatorio</div>
          <div slot="min" class="alert alert-danger mt-1">La edad minima permitida es de {{edadMin}} años</div>
          <div slot="max" class="alert alert-danger mt-1">La edad maxima permitida es de {{edadMax}} años</div>
        </field-messages>
      </validate>

      <validate tag="div">
        <!-- elemento de entrada -->
        <label for="email">Email</label>
        <input
          type="email" 
          id="email"
          name="email"
          class="form-control"
          autocomplete="off"
          v-model.trim="formData.email" 
          required          
        />
  
        <field-messages name="email" show="$dirty">
          <div slot="required" class="alert alert-danger mt-1">Este campo es obligatorio</div>
          <div slot="email" class="alert alert-danger mt-1">Email no  valido</div>
          
        </field-messages>
      </validate>
      <button class="btn btn-primary my-3" :disabled="formState.$invalid">Enviar</button>
    </vue-form>
    <div v-if="personas.length" class="table-responsive">
      <table class="table table-dark">
        <tr>
          <th>Nombre</th>
          <th>Edad</th>
          <th>Email</th>
        </tr>
        <tr v-for="(persona, index) in personas" :key="index">
          <td>{{persona.nombre}}</td>
          <td>{{persona.edad}}</td>
          <td>{{persona.email}}</td>
        </tr>
      </table>
    </div>
  </section>

</template>

<script lang="js">

  export default  {
    name: 'src-components-formulario',
    props: [],
    mounted () {

    },
    data () {
      return {
        formData: {},
        formState: {},
        nombreMinLength: 5,
        nombreMaxLength: 15,
        edadMin: 18,
        edadMax: 120,
        personas: [],
      }
    },
    methods: {
      getInicialData(){
        return{
          nombre:'',
          edad:'',
          email:'',
        }
      },
      enviar(){
        let persona = {
          "nombre": this.formData.nombre,
          "edad": this.formData.edad,
          "email": this.formData.email,
        }
        this.personas.push(persona)
        console.log(this.personas);
        this.formData = this.getInicialData()
        this.formState._reset()
      }
    },
    computed: {

    }
}


</script>

<style scoped lang="css">
  .src-components-formulario {

  }
</style>
