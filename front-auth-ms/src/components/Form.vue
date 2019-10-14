<template>
  <div class="Form">
    <h1>Formulario de autenticacion</h1>
    <form @submit.prevent="">
      Nombre: <input type="text" v-model="firstName"><br>
      Apellido: <input type="text" v-model="lastName"><br>
      Usuario: <input type="text" v-model="username"><br>
      Contraseña: <input type="password" v-model="password"><br>
      <button v-on:click=" sendWithGraphQL">Crear con GraphQL</button>
      <button v-on:click="sendWithRest">Crear con REST</button>
    </form>
    <p v-if="error">Error: Existe algun campo incompleto. Por favor, revise el formulario y oprima el boton de nuevo</p>
    <p>{{message}}</p>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name:'Form',
  data(){
    return{
      firstName: "",
      lastName: "",
      username: "",
      password: "",
      message: "No se ha creado  un nuevo usuario",
      error: false
    }
  } , 
  methods:{
    sendWithGraphQL(){
      const {firstName,lastName,username,password, message} = this;
      let respose = "";
      console.log(process.env.GRAPH_URL);
      fetch('http://localhost:5000/graphql',{
        method: 'POST',
        body: JSON.stringify({query:`mutation{createUser(user:{firstName:"${firstName}" lastName:"${lastName}" username:"${username}" password:"${password}"}){id firstName lastName username password}}`}),
        headers: {
          'Content-Type' : 'application/json',
          'Accept': 'application/json',
        }
      })
        .then(res => res.json())
        .catch(err => console.error(err))
        .then(data => this.message = ("Usuario creado satisfactoriamente desde API Gateway: Nombre del usuario: " + JSON.stringify(data.data.createUser.firstName) + " " + JSON.stringify(data.data.createUser.lastName)+ ", Username: "+ JSON.stringify(data.data.createUser.username)))
        //this.message = ("Usuario creado satisfactoriamente desde API Gateway:" + respose);
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
