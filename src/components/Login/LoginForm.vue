<template>
  <div id="fondo">
    <div class="wrapper">
      <div class="title">
        <span>Iniciar Sesión</span>
        <div class="text-subtitle2">Bienvenido</div>
      </div>

      <form action="#">
        <div class="row">
          <i class="fas fa-user"></i>
          <input v-model="email" type="text" placeholder="Username" required />
        </div>
        <div class="row">
          <i class="fas fa-lock"></i>
          <input
            v-model="password"
            type="password"
            placeholder="Password"
            required
          />
        </div>
        <div class="pass"><a href="#">¿Olvidaste tu contraseña?</a></div>
        <div class="row button">
          <input type="button" value="Login" @click="iniciarSesion" />
        </div>
        <div class="signup-link">
          ¿No tienes cuenta?
          <router-link to="#">Regístrate ahora</router-link>
        </div>
      </form>
    </div>
  </div>
</template>

<style scoped>
@import url("https://fonts.googleapis.com/css2?family=Poppins:wght@400;500;600;700&display=swap");

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: "Poppins", sans-serif;
}

#logo {
  background: linear-gradient(135deg, #1d4e98, #1abc9c);
  width: 100%; /* Ajusta el logo al ancho del contenedor */
  height: 100%; /* Ajusta el logo al alto del contenedor */
  display: flex; /* Usa flexbox para alinear el contenido */
  justify-content: center; /* Centra el contenido horizontalmente */
  align-items: center; /* Centra el contenido verticalmente */
}

#fondo {
  background-color: #1abc9c;
  background-size: cover; /* Hace que el fondo cubra toda la pantalla */
  background-position: center center; /* Centra el fondo en la pantalla */
  background-attachment: fixed; /* Mantiene el fondo fijo al hacer scroll */
  height: 100vh; /* Asegura que el fondo cubra toda la altura de la ventana */
  width: 100%; /* Asegura que el fondo cubra todo el ancho de la pantalla */
  position: fixed; /* Fija el fondo para que no se mueva con el contenido */
  top: 0;
  left: 0;
}

body {
  display: flex;
  align-items: center;
  justify-content: center;
  height: 100vh;
  padding: 15px;
  background: linear-gradient(135deg, #1d4e98, #1d4e98);
  overflow: hidden;
}

.wrapper {
  max-width: 420px;
  width: 100%;
  background: #fff;
  border-radius: 10px;
  box-shadow: 0 8px 20px rgba(0, 0, 0, 0.15);
  overflow: hidden;
  margin: 6% auto;
}

.wrapper .title {
  padding-top: 7%;
  height: 140px;
  background: linear-gradient(135deg, #1d4e98, #1abc9c);
  color: #fff;
  font-size: 28px;
  font-weight: 600;
  display: block;
  align-items: center;
  justify-content: center;
}

.title {
  text-align: center;
  font-size: 28px;
  font-weight: 600;
  margin-bottom: 15px;
}

.text-subtitle2 {
  font-size: 18px;
  color: #ffffff;
  margin-top: 5px; /* Espacio entre los textos */
}

.wrapper form {
  padding: 30px;
}

.wrapper form .row {
  height: 50px;
  margin-top: 15px;
  position: relative;
}

.wrapper form .row input {
  height: 100%;
  width: 100%;
  outline: none;
  padding-left: 55px;
  border-radius: 25px;
  border: 1px solid #ddd;
  font-size: 16px;
  transition: border 0.3s;
}

form .row input:focus {
  border-color: #1d4e98;
}

form .row input::placeholder {
  color: #bbb;
}

.wrapper form .row i {
  position: absolute;
  left: 15px;
  top: 50%;
  transform: translateY(-50%);
  color: #1d4e98;
  font-size: 20px;
}

.wrapper form .pass {
  margin-top: 10px;
  text-align: right;
}

.wrapper form .pass a {
  color: #1d4e98;
  font-size: 14px;
  text-decoration: none;
}

.wrapper form .pass a:hover {
  text-decoration: underline;
}

.wrapper form .button input {
  margin-top: 20px;
  color: #fff;
  font-size: 18px;
  font-weight: 500;
  border-radius: 25px;
  padding: 10px;
  background: linear-gradient(135deg, #1d4e98, #16a085);
  border: none;
  cursor: pointer;
  transition: background 0.3s;
}

form .button input:hover {
  background: #1d4e98;
}

.wrapper form .signup-link {
  text-align: center;
  margin-top: 25px;
  font-size: 15px;
}

.wrapper form .signup-link a {
  color: #1d4e98;
  text-decoration: none;
}

form .signup-link a:hover {
  text-decoration: underline;
}
</style>

<script>
export default {
  name: "LoginForm",
  data() {
    return {
      email: "",
      password: "",
    };
  },
  methods: {
    iniciarSesion() {
      if (!this.$api) {
        console.error("Error: $api no está definido");
        return;
      }

      const endpointURL = "/api/v1/user/signin";
      const user = {
        email: this.email,
        password: this.password,
      };
      console.log(user);
      this.$api
        .post(endpointURL, user)
        .then((response) => {
          console.log("Respuesta del servidor:", response);
          var tokenAux =
          'eyJhbGciOiJIUzI1NiJ9.eyJhdWQiOiJhMGVkY2NmYjY5ZGFmYTljMzcwYWE3NGY4NmFhMDk5ZSIsIm5iZiI6MTczMjE1OTQzMC4wNTk2NTExLCJzdWIiOiI2NzNlYTQzZmFkZTkzMTBmM2ZkZjk1ZWQiLCJzY29wZXMiOlsiYXBpX3JlYWQiXSwidmVyc2lvbiI6MX0.uVuMOkpbtZ03jV6r2i4qvkqpaQMoy9AQKNGTI6dQOII'
          //Save response.data in LocalStorage
          localStorage.setItem("userData",JSON.stringify(tokenAux)); //JSON.stringify(response.data)

          this.$q.notify({
            message: "Inicio exitoso",
            color: "positive",
            timeout: 3000,
            position: "top",
          });
          this.$router.push("/p2");
        })
        .catch((error) => {
          console.log("Esto es error", error);
          this.$q.notify({
            message: "Las credenciales son incorrectas",
            color: "negative",
            timeout: 3000,
            position: "bottom",
          });
        });
    },
  },
};
</script>
