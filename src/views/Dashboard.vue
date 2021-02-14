<template>
	<v-app id="dashboard">
		<v-app-bar app color="primary" flat>
			<v-tabs centered class="ml-n9" color="white">
				<v-tab v-for="link in links" :key="link.id" @click="getComponent(link.id)" >
					{{ link.name }}
				</v-tab>
			</v-tabs>
		</v-app-bar>
		<v-main class="grey lighten-3">
			<v-container>
				<v-row>
					<v-col cols="12" sm="1">
						<v-sheet rounded="lg" >
							<!--  -->
						</v-sheet>
					</v-col>
					<v-col cols="12" sm="10">
						<v-sheet min-height="70vh" rounded="lg" class="text-center">
							<New v-if="keyComponent==1"/>
                            <Available v-if="keyComponent==2"/>
                            <Assigned v-if="keyComponent==3"/>
						</v-sheet>
					</v-col>
					<v-col cols="12" sm="1">
						<v-sheet rounded="lg" >
							<!--  -->
						</v-sheet>
					</v-col>
				</v-row>
			</v-container>
		</v-main>
	</v-app>
</template>

<script>
    import New from '../components/New'
    import Assigned from '../components/Assigned'
    import Available from '../components/Available'
    
    export default {
        name: 'dashboard',
        components: {
            'New': New,
            'Assigned': Assigned,
            'Available': Available,
        },
        data() {
            return{
                links: [ 
                    { name:'Agendar Cita', id:1 },
                    { name:'Citas Disponibles', id:2 },
                    { name:'Citas Agendadas', id:3 }
                ],
                keyComponent: 1,
            }
        },
        methods: {
            getComponent(id) {
                console.log('lel',id)
                this.keyComponent = id
            }
        }
    }
</script>