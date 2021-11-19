<template>


	<fieldset>	
		<legend>Horários</legend>

		<input v-model="time" type="radio" value="6:00">
		<label>amanhecer</label>

		<input v-model="time" type="radio" value="10:00">
		<label>dia</label>

		<input v-model="time" type="radio" value="16:00">
		<label>entardecer</label>

		<input v-model="time" type="radio" value="19:00">
		<label>noite</label>
	</fieldset>

	<fieldset>	
		<legend>Estação do ano</legend>

		<input v-model="season" type="radio" id="notify-on" name="notify" value="primavera">
		<label>primavera</label>

		<input v-model="season" type="radio" id="notify-on" name="notify" value="verao">
		<label>verão</label>

		<input v-model="season" type="radio" id="notify-on" name="notify" value="outono">
		<label>outono</label>

		<input v-model="season" type="radio" id="notify-on" name="notify" value="inverno">
		<label>inverno</label>
	</fieldset>
	
	<p>{{ time }}</p>

	<div>
		<p>{{ vozes }}</p>	
	</div>

</template>




<script>
import axios from 'axios';

const $axios = axios.create({
	baseURL: 'https://birdscapes.herokuapp.com'
});

export default {
	mounted() {	 
		this.getData();
	},
	data() {
		return {
			time: '10:00',
			season: 'primavera',
			position: '100',
			imgs: [],
			popular: [],
			latim: [],
			ingles: [],
			audio_passaro: '',
			tudo: []
		}
	},

	methods: {
		async getData() {
			const { data } = await $axios.post('/success', {
				season: 'inverno',
				time: '11:00',
				position: '200'
			});
			this.tudo = data;
		},
	},

	computed: {
		vozes() {
			return this.tudo.vozes;
		}
	}

}


</script>



<style>
	#musgoEscuro {
		background-color: #c6cd91
	}
	#musgoClaro {
		background-color: #e6eea3
	}
	body {
		background-color: #e6eea3
	}
</style>
