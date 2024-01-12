<template>
    <div class="juego">
        <img class="imagen" v-if="imagen !== null" :src="imagen" alt="">
        <img class="imagen" v-else src="https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/dream-world/1.svg" alt="Nada">
        <label v-if="nombre !== null" for="">{{nombre}}</label>
        <label v-else for="">XXXXXXXXXXXXXXXXXXXXX</label>
    </div>
</template>

<script>
    export default {
        props:{
            objeto: null,
        },
        data() {

            return {
                nombre: null,
                imagen: null,
            }
        },
        methods: {
            asignar(){
                const {answer,image} = this.objeto;
                this.nombre = answer;
                this.imagen = image;
            }
        },
        watch:{
            objeto(viejo,nuevo){
                this.asignar();
            }
        }
    }
</script>

<style scoped>

</style>