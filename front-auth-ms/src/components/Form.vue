<template>
  <div class="Form">
    <h1>Formulario de autenticacion</h1>
    <form @submit.prevent="">
      Nombre: <input type="text" v-model="firstName"><br>
      Apellido: <input type="text" v-model="lastName"><br>
      Usuario: <input type="text" v-model="username"><br>
      Contraseña: <input type="password" v-model="password"><br>
      <button v-on:click="sendWithGraph">Crear con GraphQL</button>
      <button v-on:click="sendWithRest">Crear con REST</button>
    </form>
    <p v-if="error">Error: Existe algun campo incompleto. Por favor, revise el formulario y oprima el boton de nuevo</p>
    <p>{{firstName}}</p>
  </div>
</template>

<script>
export default {
  name:'Form',
  data(){
    return{
      firstName: "",
      lastName: "",
      username: "",
      password: "",
      error: false
    }
  } , 
  methods:{
    sendWithGraph(){
      console.log(`sending with graphQL:${this.firstName}, ${this.lastName}, ${this.username}, ${this.password}`)
    },
    sendWithRest(){
      if((this.firstName == "")||(this.lastName == "")||(this.username == "") ||(this.password == "") ){
        this.error = true; 
      }else{
        this.error = false;

        const axios = require('axios').default;

        axios.post('http://localhost:4000/sa-auth-ms/resources/users', {
          firstName: this.firstName,
          lastName: this.lastName,
          username: this.username,
          password: this.password
        })
        .then(function (response) {
          console.log(response);
        })
        .catch(function (error) {
          console.log(error);
        });

        this.firstName = this.lastName = this.username = this.password = "";

      }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
