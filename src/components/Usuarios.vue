<template>

  <section class="src-components-usuarios">
    <div class="jumbotron">
      <h1>Usuarios</h1>

      <button class="bt btn-success my-3 mr-3" @click="getUsuariosXHRpromise()"  >Pedir XHR (promise)</button>
      <button class="bt btn-success my-3 mr-3" @click="getUsuariosFetch()"  >Pedir Fetch</button>
      <button class="bt btn-success my-3 mr-3" @click="getUsuariosAxios()"  >Pedir Axios</button>
      <button class="bt btn-danger" my-3 @click="personas = []">CLEAR</button>

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
    </div>  
  </section>

</template>

<script>

  export default  {
    name: 'src-components-usuarios',
    props: [],
    mounted () {

    },
    data () {
      return {
        url: 'https://62b279fd20cad3685c8e74df.mockapi.io/personas',
        personas: [],
      }
    },
    methods: {
      wrapperXHRpromise(){
          return new Promise((resolve, reject)=>{
            const xhr = new XMLHttpRequest()
            xhr.open('get', this.url)
            xhr.addEventListener('load', () => {
              if(xhr.status == 200){
                let respuesta = JSON.parse(xhr.response)
                //console.log(respuesta)
                resolve(respuesta)
              }
              else{
                console.log('ERROR XHR cb (status)', xhr.status)
                let error = {
                  title: 'ERROR XHR cb (status)',
                  status: xhr.status
                }
                reject(error)
              }
            })
            xhr.addEventListener('error', e=>{
              console.log('ERROR XHR cb (ajax)', e)

              let error = {
                  title: 'ERROR XHR cb (ajax)',
                  status: e
                }
                reject(error)
            })
            xhr.send()
          })
      },
      async getUsuariosXHRpromise(){
        try {
          let respuesta =  await this.wrapperXHRpromise()
          this.personas = respuesta
        } catch (error) {
          console.log('Error XHRpromise', error);
        }
      },
      async getUsuariosFetch(){
        try {
          let response = await fetch(this.url)
          let respuesta = await response.json()
          this.personas = respuesta
        } catch (error) {
          console.log('Error Fetch', error);
        }
      },
      async getUsuariosAxios(){
        try {
          let {data} = await this.axios(this.url)
          this.personas = data
        } catch (error) {
          console.log('Error Axios', error);
        }
      }
    },
    computed: {

    }
}


</script>

<style scoped lang="css">
  .src-components-usuarios {

  }
</style>
