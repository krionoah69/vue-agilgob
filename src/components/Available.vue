<template>
    <v-app>
        <h2> Citas Disponibles</h2>
        <v-container>
            <v-row>
              <v-col cols="6" sm="6">
                        <v-text-field
                            label="date"
                            single-line
                            type="date"
                            v-model="dateSelect"
                        ></v-text-field>
                    </v-col>
                    <v-col cols="6" sm="6">
                        <v-btn
                            class="ma-2"
                            outlined
                            color="secondary"
                            @click="onSelect"
                        >
                            Filtrar
                        </v-btn>
                        <v-btn
                            class="ma-2"
                            outlined
                            color="primary"
                            @click="onClean"
                        >
                            Sin Filtro
                        </v-btn>
                    </v-col>
            </v-row>
            <v-data-table
                :headers="headers"
                :items="desserts"
                :items-per-page="5"
                class="elevation-1"
            ></v-data-table>
        </v-container>
    </v-app>
</template>
<script>
  export default {
    data () {
      return {
        headers: [
          { text: 'ID', align: 'start', value: 'id' },
          { text: 'Sede', value: 'sede' },
          { text: 'Asesor', value: 'asesor' },
          { text: 'Hora Inicio', value: 'start' },
          { text: 'Hora Final', value: 'end' },
          { text: 'Fecha', value: 'date' },
        ],
        desserts: [],
        dateSelect: null
      }
    },
    methods: {
      onClean() {
            this.getData()
            this.dateSelect = null
        },
        onSelect() {
            this.getDataFiltered(this.dateSelect)
        },
        getData() {
          this.axios.get('http://localhost:8000/api/getOrder')
          .then((response) => {
            var data = response.data
            var tickets = []

            for(var i = 0; i < data.length; i++) {
              tickets.push({
                id: data[i]['id'],
                sede: data[i]['sede_id'],
                asesor: data[i]['user_id'],
                start: data[i]['start_time'],
                end: data[i]['end_time'],
                date: data[i]['date'],
              })
            }
            this.desserts = tickets
          })
          .catch((error) => {
              console.log('gg',error.response)
          })
        },
        getDataFiltered(date) {
          this.axios.get('http://localhost:8000/api/getFiltered',{params: {date: date}})
          .then((response) => {
            console.log('f',response.data)
            var data = response.data
            var tickets = []

            for(var i = 0; i < data.length; i++) {
              tickets.push({
                id: data[i]['id'],
                sede: data[i]['sede_id'],
                asesor: data[i]['user_id'],
                start: data[i]['start_time'],
                end: data[i]['end_time'],
                date: data[i]['date'],
              })
            }
            this.desserts = tickets
          })
          .catch((error) => {
              console.log('gg',error.response)
          })
        }
    },
    created() {
        this.getData()
    }
  }
</script>