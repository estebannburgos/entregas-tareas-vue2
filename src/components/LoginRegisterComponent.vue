<template>
  <div class="container">
    <div class="login-register">
      <div class="login" v-show="showLogin">
        <h2>Iniciar sesión</h2>
        <p><small>Para iniciar sesión, usa la siguiente credencial:</small></p>
        <p><small>Usuario: user1; Contraseña: test123</small></p>
        <div class="form-group">
          <input v-model="loginData.username" type="text" id="username" class="form-input" placeholder="Usuario">
        </div>
        <div class="form-group">
          <input v-model="loginData.password" type="password" id="password" class="form-input" placeholder="Contraseña">
        </div>
        <button @click="login">Iniciar sesión</button>
        <p>¿No tienes una cuenta aún? <button @click="switchToRegistration">Regístrate aquí</button></p>
        <div v-if="loginError" class="alert alert-danger">{{ loginError }}</div>
      </div>

      <div class="registration" v-show="showRegistration">
        <h2>Registrarse</h2>
        <div class="form-group">
          <input v-model="registerData.nombre" type="text" id="nombre" class="form-input" placeholder="Nombre">
        </div>
        <div class="form-group">
          <input v-model="registerData.apellido" type="text" id="apellido" class="form-input" placeholder="Apellido">
        </div>
        <div class="form-group">
          <input v-model="registerData.usuario" type="text" id="usuario" class="form-input" placeholder="Usuario">
        </div>
        <div class="form-group">
          <input v-model="registerData.email" type="email" id="email" class="form-input" placeholder="Email">
        </div>
        <div class="form-group">
          <input v-model="registerData.password" type="password" id="password" class="form-input" placeholder="Contraseña">
        </div>
        <button @click="register">Registrarse</button>
        <p>¿Ya tienes una cuenta? <button @click="switchToLogin">Inicia sesión aquí</button></p>
        <div v-if="registrationError" class="alert alert-danger">{{ registrationError }}</div>
      </div>
    </div>
  </div>
</template>

<script>
import users from "@/assets/database/users.json";

export default {
  data() {
    return {
      loginData: {
        username: "",
        password: "",
      },
      registerData: {
        nombre: "",
        apellido: "",
        usuario: "",
        email: "",
        password: "",
      },
      loginError: "",
      registrationError: "",
      showLogin: true,
      showRegistration: false,
    };
  },
  methods: {
    login() {
      if (!this.loginData.username || !this.loginData.password) {
        this.loginError = "Por favor, ingresa el usuario y la contraseña.";
        return;
      }

      const user = users.find(
        (u) =>
          u.username === this.loginData.username &&
          u.password === this.loginData.password
      );

      if (user) {
        // Autenticación exitosa
        //this.$store.commit("login", user);
        this.loginData.username = "";
        this.loginData.password = "";
        this.loginError = "";
        window.location.href = "/products";
      } else {
        // Autenticación fallida
        this.loginError = "Credenciales de inicio de sesión incorrectas.";
      }
    },
    switchToRegistration() {
      this.showLogin = false;
      this.showRegistration = true;
    },
    register() {
      if (
        !this.registerData.nombre ||
        !this.registerData.apellido ||
        !this.registerData.usuario ||
        !this.registerData.password
      ) {
        this.registrationError = "Por favor, completa todos los campos.";
        return;
      }

      const newUser = {
        id: users.length + 1,
        firstname: this.registerData.nombre,
        lastname: this.registerData.apellido,
        username: this.registerData.usuario,
        password: this.registerData.password,
        email: this.registerData.email,
        avatar: "https://avatars.githubusercontent.com/u/583231?v=4",
        admin: false,
      };

      users.push(newUser);

      this.registerData.nombre = "";
      this.registerData.apellido = "";
      this.registerData.usuario = "";
      this.registerData.password = "";
      this.registerData.email = "";
      this.registrationError = "";

    },
    switchToLogin() {
      this.showRegistration = false;
      this.showLogin = true;
    },
  },
};
</script>

<style scoped>
.container {
  background-color: rgb(0, 156, 161);
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
}

.login-register {
  background-color: white;
  padding: 2rem;
  border-radius: 10px;
  width: 20rem;
}

.login,
.registration {
  margin-bottom: 2rem;
}

.form-group {
  margin-bottom: 1rem;
}

.label {
  margin-bottom: 0.5rem;
}

.form-input {
  width: 100%;
  padding: 0.5rem;
  border: 1px solid #ccc;
  border-radius: 4px;
}

.alert {
  color: red;
}
</style>
