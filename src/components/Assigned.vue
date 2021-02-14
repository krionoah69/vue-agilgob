<template>
    <v-app>
        <h2> Citas Agendadas</h2> 
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
          { text: 'Fecha', value: 'date' },
          { text: 'Hora', value: 'time' },
          { text: 'Sede', value: 'sede' },
          { text: 'Asesor', value: 'asesor' },
          { text: 'usuario', value: 'user' },
        ],
        desserts: [],
        dateSelect: null
      }
    },
    methods: {
        onClean() {
            this.dateSelect = null
            this.getData()
        },
        onSelect() {
            this.getData(this.dateSelect)
        },
        getData(date = null) {
            if (date != null) {
                this.axios.get('http://localhost:8000/api/getAssigned',{params: {date: date}})
                .then((response) => {
                    var data = response.data
                    var tickets = []

                    for(var i = 0; i < data.length; i++) {
                        tickets.push({
                            id: data[i]['id'],
                            date: data[i]['date'],
                            time: data[i]['start_time'] + ' - ' + data[i]['end_time'],
                            sede: data[i]['sede_id'],
                            asesor: data[i]['user_id'],
                            user: data[i]['user'],
                        })
                    }
                    this.desserts = tickets
                })
                .catch((error) => {
                    console.log('gg',error.response)
                })
            } else {
                this.axios('http://localhost:8000/api/getAssigned')
                .then((response) => {
                    var data = response.data
                    var tickets = []

                    for(var i = 0; i < data.length; i++) {
                        tickets.push({
                            id: data[i]['id'],
                            date: data[i]['date'],
                            time: data[i]['start_time'] + ' - ' + data[i]['end_time'],
                            sede: data[i]['sede_id'],
                            asesor: data[i]['user_id'],
                            user: data[i]['user'],
                        })
                    }
                    this.desserts = tickets
                })
                .catch((error) => {
                    console.log('gg',error.response)
                })
            }
        }
    },
    created() {
        this.getData()
    }
  }
</script>