<template>
  <div>
    <div class="Form">
      <h1>Formulario de autenticación</h1>
      <div class="form-container">
        <form @submit.prevent="" onkeydown="return event.key != 'Enter';">
          <span>Nombre: <input type="text" v-model="firstName"><br></span>
          <span>Apellido: <input type="text" v-model="lastName"><br></span>
          <span>Usuario: <input type="text" v-model="username"><br></span>
          <span>Contraseña: <input type="password" v-model="password"><br></span>
        </form>
      </div>
      <button v-on:click=" sendWithGraphQL">Crear con GraphQL</button>
      <button v-on:click="sendWithRest">Crear con REST</button>
    </div>
    <p v-if="error">Error: Existe algun campo incompleto. Por favor, revise el formulario y oprima el boton de nuevo</p>
      <div v-if="created">
        <h2>Usuario creado satisfactoriamente desde {{component}}:</h2>
        <ul>
          <li>Nombre: {{responseFirstName}}</li>
          <li>Apellido: {{responseLastName}}</li>
          <li>Usuario: {{responseUsername}}</li>
        </ul>
      </div>
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
      responseFirstName: "",
      responseLastName: "",
      responseUsername: "",
      component:"",
      error: false,
      created: false,
    }
  } , 
  methods:{
    sendWithGraphQL(){
      if((this.firstName == "")||(this.lastName == "")||(this.username == "") ||(this.password == "")){
        this.error = true;
        this.created = false;
      }else{
        this.error = false;
        const {firstName,lastName,username,password} = this;
        let respose = "";
        fetch("http://localhost:5000/graphql",{
          method: 'POST',
          body: JSON.stringify({query:`mutation{createUser(user:{firstName:"${firstName}" lastName:"${lastName}" username:"${username}" password:"${password}"}){id firstName lastName username password}}`}),
          headers: {
            'Content-Type' : 'application/json',
            'Accept': 'application/json',
          }
        })
          .then(res => res.json())
          .catch(err => console.error(err))
          .then(data =>{
            this.firstName = this.lastName = this.username = this.password = "";
            this.component = "API Gateway";

            this.responseFirstName = data.data.createUser.firstName;
            this.responseLastName = data.data.createUser.lastName;
            this.responseUsername = data.data.createUser.username;

            this.created =true;

          } )
        }
    },
    sendWithRest(){
      if((this.firstName == "")||(this.lastName == "")||(this.username == "") ||(this.password == "") ){
        this.error = true; 
        this.created = false;
      }else{
        this.error = false;

        const axios = require('axios').default;

        axios.post('http://localhost:4000/sa-auth-ms/resources/users', {
          firstName: this.firstName,
          lastName: this.lastName,
          username: this.username,
          password: this.password
        })
        .then( (response) => {
          this.firstName = this.lastName = this.username = this.password = "";

          this.component = "microservicio";

          this.responseFirstName = response.data.firstName;
          this.responseLastName = response.data.lastName;
          this.responseUsername = response.data.username;

          this.created =true;
        })
        .catch((error) => {
          console.log(error);
        });
      }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
ul {
  list-style-type: none;
  justify-items: start;
  padding: 0;
}
li {
  display: default;
  margin: 0 10px;
}
.Form {
  text-align: center;
  background-color: lightgrey;
  width: 400px;
  margin: 0 auto;
}
.form-container{
  text-align: end;
  width: 300px;
}
button {
  margin: 10px 10px;
}
</style>
