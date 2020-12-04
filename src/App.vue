<template>
  <div id="app">
    <h1 class="text-center">Piedra - Papel - Tijera</h1>
    <!-- <p class="text-center">Selecciona una Opcion</p> -->
    <div class="container">
      <table>
        <thead>
          <th>Maquina</th>
          <th>Usuario</th>
        </thead>
        <tbody>
          <tr>
            <td>{{pcMarcador}}</td>
            <td>{{usuarioMarcador}}</td>
          </tr>
        </tbody>
      </table>
      <div class="elecciones">
        <div v-on:click="seleccionarOpcion(1)"
             class="eleccion"
             v-bind:class="{
               'eleccion-usuario': this.usuario === 1,
               'eleccion-pc': this.pc === 1,
             }">
          <img src="./assets/puño.png" alt="">
        </div>
        <div v-on:click="seleccionarOpcion(2)"
             class="eleccion"
             v-bind:class="{
               'eleccion-usuario': this.usuario === 2,
               'eleccion-pc': this.pc === 2,
             }">
          <img src="./assets/papel.png" alt="">
        </div>
        <div v-on:click="seleccionarOpcion(3)"
             class="eleccion"
             v-bind:class="{
               'eleccion-usuario': this.usuario === 3,
               'eleccion-pc': this.pc === 3,
             }">
          <img src="./assets/tijera.png" alt="">
        </div>
      </div>
      <div class="error" v-if="this.error">
        <p>{{ error }}</p>
      </div>
      <div v-if="!this.empezado" class="btn-empezar">
        <button v-on:click="empezar()">Empezar</button>
      </div>
      <div v-if="this.empezado" class="btn-empezar">
        <button v-on:click="reintentar()">Volver a Empezar</button>
      </div>

    </div>
  </div>
</template>

<script>


export default {
  name: 'App',
  data() {
    return {
      error: null,
      resultado: null,
      piedra: 1,
      papel: 2,
      tijera: 3,
      usuario: null,
      usuarioMarcador: 0,
      pc: null,
      pcMarcador: 0,
      empezado: false,
    }
  },
  methods: {
    empezar() {

      this.error = null;

      this.pc = this.generarNumeroAleatorio(1, 4);

      console.log(`El Juego empezó. PC: ${this.pc} USUARIO: ${this.usuario}`);

      if (this.usuario === this.pc) {
          this.empezado = true;

      } else if ((this.usuario === this.piedra && this.pc === this.papel) || (this.usuario === this.tijera && this.pc === this.piedra) || (this.usuario === this.papel && this.pc === this.tijera)) {
          this.empezado = true;
          this.pcMarcador = this.pcMarcador + 1;

      } else if((this.usuario === this.papel && this.pc === this.piedra) || (this.usuario === this.piedra && this.pc === this.tijera) || (this.usuario === this.tijera && this.pc === this.papel)) {
          this.empezado = true;
          this.usuarioMarcador = this.usuarioMarcador + 1;

      } else {
          this.empezado = false;
          this.error = "Selecciona una de las Opciones";
      }

    },

    reintentar() {
      this.empezado = false;
      this.pc = null;
      this.usuario = null;
      this.error = null;
    },

    seleccionarOpcion(opcion) {
      if (!this.empezado) {
        this.usuario = opcion;
      }
    },

    generarNumeroAleatorio(min, max) {
      return Math.floor(Math.random() * (max - min)) + min;
    }
  }
}


</script>

<style>
html,
body {
  width: 100%;
  height: 100%;
  background-image: url("./assets/background.jpg");
  background-repeat: no-repeat;
  background-size: cover;
}
* {
  margin: 0;
  padding: 0;
}

#app {
  font-family: monospace;
  color: white;
  width: 500px;
  margin: 0 auto;
}
.text-center {
  text-align: center;
}
.text-uppercase {
  text-transform: uppercase;
}

h1 {
  padding: 100px 0px;
}

.container {
  width: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}
table {
  width: 100%;
  text-align: center;
  font-size: 20px;
}

table, th, td {
  border: 1px solid white;
  border-collapse: collapse;
  padding: 10px;
}
.elecciones {
  width: 100%;
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-gap: 40px;
  justify-content: center;
  align-items: center;
}

.eleccion {
  margin-top: 50px;
}
.eleccion img {
  height: 120px;
  width: 120px;
  font-size: 25px;
  cursor: pointer;
}
.eleccion-pc {
  position: relative;
  border: 1px solid #fff;

}
.eleccion-pc::before {
  position: absolute;
  content: "Maquina";
  background-color: red;
  color: white;
  right: 0;
}

.eleccion-usuario {
  position: relative;
  border: 1px solid #fff;

}

.eleccion-usuario::after {
  position: absolute;
  content: "Usuario";
  background-color: red;
  color: white;
  left: 0;
}

.btn-empezar {
  width: 100%;
  display: flex;
  justify-content: center;
  align-items:center;
  margin-top: 60px;
}

.btn-empezar button {
  color: white;
  background-color: transparent;
  padding: 10px 10px;
  font-weight: bold;
  border: 1px solid white;
  width: 100%;
  font-size: 16px;
  cursor: pointer;
  transition: all 0.3s ease;

}

.error {
  margin-top: 40px;
  color: red;
}

</style>
