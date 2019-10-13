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
  </div>
</template>

<script>
export default {
  name:'Form',
  data(){
    return{
      firstName: null,
      lastName: null,
      username: null,
      password: null
    }
  } , 
  methods:{
    sendWithGraph(){
      console.log(`sending with graphQL:${this.firstName}, ${this.lastName}, ${this.username}, ${this.password}`)
    },
    sendWithRest(){
      console.log(`sending with rest:${this.firstName}, ${this.lastName}, ${this.username}, ${this.password}`)
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
form {
  
}
</style>
