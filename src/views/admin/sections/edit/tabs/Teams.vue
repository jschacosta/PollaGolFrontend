<template>
    <div >
        <h4 class="mb-3">En esta pestaña, se ingresan los equipos para cada torneo ( Un equipo puede estar en varias competiciones, ya sea selección o club). Además se pueden borrar los equipos que de cada torneo.</h4>
        <h4>Estos equipos son usados para la sección "Crear Partidos".</h4>
        <br>
        <v-row>
            <!-- Columna 1 con buscador -->
            <v-col cols="12" md="6">
                <v-row v-if="busqueda===false">
                    <v-btn  class="ml-4" dark color="indigo darken-4" @click="mostrar()">
                    <v-icon>mdi-chevron-down</v-icon>Mostrar Menú
                    </v-btn> 
                </v-row>
                <v-row v-if="busqueda===true">
                    <v-btn class="ml-4" dark color="grey darken-4" @click="mostrar()"> 
                        <v-icon>mdi-chevron-up</v-icon>Ocultar Menu
                    </v-btn>
                </v-row>
                <!-- Componente Formulario Equipos -->
                <formEquipos  v-if="busqueda===true"></formEquipos>
            </v-col>
            <!-- Columna 2 con Tabla -->
            <v-col sm="12" md="6">
                    <!-- Componente Tabla -->
                   <tablaEquipos></tablaEquipos>        
            </v-col>
        </v-row>
        <!-- Dialog para Agregar Nuevo Equipo -->   
  </div>
</template>

<script>
import formEquipos from '@/components/forms/selectForm/formEquipos';
import tablaEquipos from '@/components/tables/tablaEquipos'
import { mapMutations, mapState } from 'vuex';
export default {
    name: 'Teams',
    components: {
        formEquipos,
        tablaEquipos
    },
    data(){
    
        return{  
            busqueda:true,
        }
    },
    methods: {
        mostrar(){
            this.busqueda=!this.busqueda   
        }
    }
}
</script>
