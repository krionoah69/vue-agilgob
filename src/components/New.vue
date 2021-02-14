<template>
    <v-app>
        <h2> Agregar Cita </h2>
        <v-card
            class="mx-auto my-12"
            max-width="374"
        >
            <v-card-title>Proxima cita</v-card-title>
            <v-card-text>
                <v-row align="center" class="mx-0">
                    <div class="grey--text">{{ this.date }}</div>
                </v-row>
                <div class="my-4 subtitle-1">
                    Sede {{ this.cita.sede_id }}
                </div>
                <div>La cita {{this.cita.id}} es la mas proxima a atender, ingrese el usuario para agendar esta cita.</div>
                <v-col
                    cols="12"
                    sm="12"
                    md="12"
                >
                    <v-text-field
                        label="Usuario"
                        placeholder="ID de usuario"
                        v-model="user"
                    ></v-text-field>
                </v-col>
                <v-alert
                v-if="error != ''"
      border="top"
      color="red lighten-2"
      dark
    >
      {{this.error}}
    </v-alert>
            </v-card-text>
            <v-divider class="mx-4"></v-divider>
            <v-card-title>Disponibilidad de horarios</v-card-title>
            <v-card-text>
                <v-chip-group
                    active-class="deep-purple accent-4 white--text"
                    column
                >
                <v-chip>{{this.cita.start_time}}</v-chip>
                <v-chip>a</v-chip>
                <v-chip>{{this.cita.end_time}}</v-chip>
            </v-chip-group>
        </v-card-text>
        <v-card-actions>
        <v-btn
            color="deep-purple lighten-2"
            text
            @click="createTicket"
        >
            Asignar cita
        </v-btn>
    </v-card-actions>
  </v-card>
    </v-app>
</template>
<script>
  export default {
    data () {
      return {
        cita: [],
        user: '',
        date: null,
        error: ''
      }
    },
    methods: {
        getData() {
          this.axios.get('http://localhost:8000/api/getFirst')
          .then((response) => {
            var data = response.data
            this.cita = data
            var objFecha = new Date(data.date);
            var dia  = objFecha.getDate();
            var mes  = objFecha.getMonth() + 1;
            var anio = objFecha.getFullYear();
            this.date = anio + '-' + mes + '-' + dia
          })
          .catch((error) => {
              console.log('gg',error.response)
          })
        },
        createTicket() {
            this.error = ''
            if (this.user != '') {
                this.axios.put('http://localhost:8000/api/appointments/update/'+this.cita.id,{user: this.user})
                .then((response) => { 
                    if (response.data['errors']) {
                        this.error = response.data['errors']['user'][0]
                    }
                    this.getData()
                    this.user = ''
                })
                .catch(() => {
                    this.error = '¡user error!'
                })
            } else {
                this.error = '¡empty text field!'
            }
        }
    },
    created() {
        this.getData()
    }
  }
</script>