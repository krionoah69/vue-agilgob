<template>
	<v-app>
		<router-view/>
		<v-footer dark color="secondary">
			<v-spacer></v-spacer>
			<div>Copyright  &copy; {{ new Date().getFullYear() }} Agilgob.</div>
		</v-footer>
	</v-app>
</template>

<script>
	export default {
		name: 'App'
	}
</script>