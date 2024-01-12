<template>
  <div v-if="!reiniciar" class="container">
    <label for="">Puntaje : {{ puntaje }}</label>
    <label for="">Intento : {{ intentos }}</label>
  </div>

  <div class="container">
    <div v-if="!reiniciar">
      <Juego :objeto="valor1" />
      <Juego :objeto="valor2" />
      <Juego :objeto="valor3" />
      <button @click="jugar">JUGAR</button>
    </div>
    <div class="juego" v-else-if="puntaje >= 10">
      <label class="azul" for="">Puntaje: {{ puntaje }}</label>
      <label class="azul" for=""
        >Felicitaciones has ganado un premio de $10.000,00</label
      >
      <button @click="nuevo">Nuevo Juego</button>
    </div>
    <div class="juego" v-else>
      <label class="rojo" for="">Has utilizado tus 5 intentos</label>
      <label class="rojo" for=""
        >El juego ha termindo, intentalo nuevamente</label
      >
      <button @click="nuevo">Nuevo Juego</button>
    </div>
  </div>
</template>

<script>
import HelloWorld from "./components/HelloWorld.vue";
import Juego from "./components/Juego.vue";
export default {
  name: "App",
  components: {
    // HelloWorld
    Juego,
  },
  data() {
    return {
      valor1: null,
      valor2: null,
      valor3: null,
      puntaje: 0,
      intentos: 0,
      reiniciar: null,
    };
  },
  methods: {
    async consumirAPI() {
      const data = (await fetch("https://yesno.wtf/api")).json((r) => r.json());
      return data;
    },

    async jugar() {
      this.valor1 = await this.consumirAPI();
      this.valor2 = await this.consumirAPI();
      this.valor3 = await this.consumirAPI();

      // console.log(this.valor1);

      const { answer: n1 } = this.valor1;
      const { answer: n2 } = this.valor2;
      const { answer: n3 } = this.valor3;
      if (this.intentos == 5) {
        this.reiniciar = true;
      } else {
        if (n1 === "yes" && n2 === "yes" && n3 === "yes") {
          this.puntaje += 5;
        } else if (this.secundario()) {
          this.puntaje += 2;
        } else if (n1 === "yes" || n2 === "yes" || n3 === "yes") {
          this.puntaje += 1;
        }
        this.intentos++;
      }
    },
    nuevo() {
      this.puntaje = 0;
      this.intentos = 0;
      this.reiniciar = false;
    },
    secundario() {
      const { answer: n1 } = this.valor1;
      const { answer: n2 } = this.valor2;
      const { answer: n3 } = this.valor3;

      if (n1 === "yes" && n2 === "yes" && n3 !== "yes") {
        return true;
      } else if (n1 === "yes" && n2 !== "yes" && n3 === "yes") {
        return true;
      } else if (n1 !== "yes" && n2 === "yes" && n3 === "yes") {
        return true;
      } else {
        return false;
      }
    },
  },
};
</script>

<style>
/* #app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
} */
.container {
  display: flex;
  justify-content: center;
  align-items: center;
}

</style>
