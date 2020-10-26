<template>
  
  <form @submit.prevent="login" class="formulario">
    <h1>Iniciar Sesión</h1>
    <div class="contenedor"></div>
    <div>
      <label for="email" style="width:76px;">Email</label>
      <input class="input-contenedor"  type="text" v-model="email" id="email" />
    </div>
    <div>
      <label for="password" style="width:76px;">Contraseña</label>
      <input class="input-contenedor" type="password" v-model="password" id="password" />
    </div>

    <div style="color: red" v-if="error">
        Credenciales incorrectas
    </div>

    <button type="submit" class="button_form">
        Iniciar Sesión
    </button>
  </form>
</template>
<script>
export default {
  name: "Login",
  data() {
    return {
      email: "",
      password: "",
      error: false
    };
  },
  methods: {
      login(){
          this.error = false;
          fetch("http://localhost:8000/api-auth/", {
              method: 'POST',
              headers: {
                  'Content-Type': 'application/json'
              },
              body: JSON.stringify({
                  username: this.email,
                  password: this.password
              })
          }).then(async (response) => {
              if(!response.ok) throw await response.json();

              return response.json();
          })
          .then((data) => {
              localStorage.setItem('token', data.token)
              this.$router.push('/')
          }).catch(err =>{
              this.error = true;
          });
      },
  }
};
</script>
<style>
</style>
