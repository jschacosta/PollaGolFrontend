<template>
  <div class="mt-5 container">
    <v-row class="mb-5">
      <v-col :cols="imagen != 'xs' ? '2' : '4'">
        <v-btn
          rounded
          small
          color="#2C3A47"
          @click="$router.push({ path: `/` })"
          dark
          >Volver</v-btn
        >
      </v-col>
      <v-col :cols="imagen != 'xs' ? '10' : '8'">
        <h2 class="titulo" v-if="imagen != 'xs'">Presagio de {{ nickName }}</h2>
        <h3 class="titulo" v-if="imagen === 'xs'">
          Presagio de {{ nickName }}
        </h3>
      </v-col>
    </v-row>
    <v-row class="ancho" v-if="partidosOtro.length != 0">
      <v-tabs vertical class="elevation-10">
        <v-tab v-for="(partido, i) of partidosOtro" :key="i">
          P {{ i + 1 }}
        </v-tab>

        <v-tab-item v-for="(parti2, j) of partidosOtro" :key="j" class="mb-3">
          <v-card color="#E0E0E0">
    <!-- <p  class="text-center text-caption"  v-if="imagen==='xs'">Voltea el telefono para ver puntajes</p> -->

            <v-row class="d-flex justify-center align-center  eltexto mb-6" dense>
              <v-col class="d-flex justify-center  mt-5" cols="3" :class="imagen==='xs'?'text-body-2':'display'">
                {{ parti2.local }}
              </v-col>
              <v-col class="d-flex justify-center align-center mt-5" cols="1" :class="imagen==='xs'?'mx-2':'holas'">
                {{ parti2.scoreOtro[0] }}
              </v-col>
              <v-col class="d-flex justify-center align-center mt-5" cols="1">
                <h3>-</h3>
              </v-col>
              <v-col class="d-flex justify-center align-center mt-5" cols="1" :class="imagen==='xs'?'mx-2':'holas'">
                {{ parti2.scoreOtro[1] }}
              </v-col>
              <v-col class="d-flex justify-center align-center mt-5" cols="3" :class="imagen==='xs'?'text-body-2':'display'">
                {{ parti2.visita }}
              </v-col>
            </v-row>

            <div
              v-for="(item2, k) of parti2.rules"
              :key="k"
              :class="imagen==='xs'?'text-body-2':'display'"
            >
                  <h4 class="mx-5  mb-2 font-weight-regular" :class="imagen==='xs'?'text-center':'holas'">
                    {{ item2.text }}

                    <v-icon
                      right
                      v-if="
                        parti2.ruleElectionsOtro[k] ===
                          parti2.ruleResult[k] - 1 &&
                        parti2.ruleResult[k] - 1 != -1
                      "
                      color="success"
                      >fas fa-check</v-icon
                    >
                    <v-icon
                      right
                      v-if="
                        parti2.ruleElectionsOtro[k] !=
                          parti2.ruleResult[k] - 1 &&
                        parti2.ruleResult[k] - 1 != -1 &&
                        parti2.ruleResult[k] != null &&
                        parti2.ruleElectionsOtro[k] != null
                      "
                      color="error"
                      >fas fa-times</v-icon
                    >
                  </h4>
                  <v-row>
                <v-col v-if="parti2.ruleElectionsOtro[k] != null">
                  <v-radio-group v-model="parti2.ruleElectionsOtro[k]" disabled>
                    <v-row
                      dense
                      v-for="(texto, m) of parti2.rules[k].options.text"
                      :key="m"
                    >
                      <v-col v-if="parti2.ruleElectionsOtro[k] === m" >
                        <v-radio
                          class="mx-8"
                          :label="`${texto}`"
                          :value="m"
                        ></v-radio>
                      </v-col>
                      <v-col v-if="parti2.ruleElectionsOtro[k] === m ">
                         {{parti2.rules[k].options.values[m]}} pts 
                      </v-col>
                    </v-row>
                  </v-radio-group>
                </v-col>
                <v-col
                  v-if="parti2.ruleElectionsOtro[k] === null"
                  class="text-center"
                >
                  <h4 class="font-weight-regular py-5 ">N/A</h4>
                </v-col>
              </v-row>
            </div>
          </v-card>
        </v-tab-item>
      </v-tabs>
    </v-row>
    <v-container>

    <v-card max-width="600px"  class="general elevation-8 pa-0 ">
    <v-list class="pa-0 ma-0">
      <v-list-item-group>
    <v-item-group>
      <v-list-item-content v-if="partidosOtro.length === 0" class="normal">
        <v-list-item-title class="headline mb-1  text-center">
          <v-icon large left color="#2C3A47">fab fa-old-republic</v-icon>
          <p>No hay datos para mostrar</p>
        </v-list-item-title>
      </v-list-item-content>
    </v-item-group>
      </v-list-item-group>
    </v-list>
    </v-card>
    </v-container>

  </div>
</template>

<script>
import { mapMutations, mapState } from "vuex";

export default {
  name: "JugadasOtros",
  data: () => ({
    partidosOtro: [],
    nickName: "",
  }),

  computed: {
    ...mapState("torneo", ["torneos", "matchTorneo"]),
    imagen() {
      switch (this.$vuetify.breakpoint.name) {
        case "xs":
          return "xs";
        case "sm":
          return "sm";
        case "md":
          return "md";
        case "lg":
          return "lg";
        case "xl":
          return "xl";
      }
    },
    sumadeTodo() {
      const array = this.puntosSeparados.matches.reduce(
        (r, a) => a.map((b, i) => (r[i] || 0) + b),
        []
      );
      return array.reduce((a, b) => a + b, 0);
    },
  },
  methods: {
    ...mapMutations("torneo", ["cambiarDialog", "obtenerTorneos"]),
    ...mapMutations("loading", ["loadingFunction"]),
    unirse() {
      this.cambiarDialog(true);
    },
  },
  created() {
    const arrayJugador = [];
    const index = this.torneos[0].players.findIndex(
      (item) => item._id === this.$route.params.id
    );
    this.nickName = this.torneos[0].players[index].nickName;
    const matchUsuario = this.torneos[0].players[index].matches;
    for (let item of matchUsuario) {
      for (let item2 of this.matchTorneo) {
        if (item._id === item2._id && item2.estado != "POR JUGAR") {
          item2.scoreOtro = item.score;
          item2.ruleElectionsOtro = item.ruleElections;
          arrayJugador.push(item2);
        }
      }
    }
    this.partidosOtro = arrayJugador;
  },
};
</script>
<style scoped>
.titulo {
  font-family: Arial, Helvetica, sans-serif;
}
.normal {
  background-color: #e0e0e0;
  color: #2c3a47;
}
.general{
  margin-left: auto;
  margin-right: auto;
}
.eltexto{
  text-align:center ;
  text-justify: inter-word;
}
.ancho{
  margin: 0 auto;
  display: flex;
  justify-content: center;
  max-width: 800px;
}
</style>