<template>

	<button type="button" v-on:click="getData">Iniciar</button>

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

		<input v-model="season" type="radio" id="notify-on" name="notify" value="primavera"  >
		<label>primavera</label>

		<input v-model="season" type="radio" id="notify-on" name="notify" value="verao"  >
		<label>verão</label>

		<input v-model="season" type="radio" id="notify-on" name="notify" value="outono"  > 
		<label>outono</label>

		<input v-model="season" type="radio" id="notify-on" name="notify" value="inverno"  >
		<label>inverno</label>
	</fieldset>
	
	<p>{{ time }}</p>

	<div>
		<p>{{ vozes }}</p>	
	</div>

</template>




<script>
import axios from 'axios';
import {Howl} from 'howler';

const $axios = axios.create({
	baseURL: 'https://birdscapes.herokuapp.com'
});

export default {
	mounted() {	 
		//this.getData();
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
			vozes: [],
		}
	},

	methods: {
		async getData() {
			const { data } = await $axios.post('/success', {
				season: this.season,
				time: this.time,
				position: this.position
			});
			this.imgs = data.imagens;
			this.popular = data.populares;
			this.latim = data.latins;
			this.vozes = data.vozes;
			this.ingles = data.ingleses;
		},
		playVoz(voz){
			var sound = new Howl({
				src: 'https://birdscapes.herokuapp.com/voz/' + voz,
				format: ['mp3', 'aac']
			});

			sound.play();
		}
		
	},

	watch: {
		season: function() { this.getData() },
		time: function() { this.getData() },
		vozes: function() { this.playVoz(this.vozes[0]) }
	},


//	computed: {
//		vozes() {
//			return this.data.vozes;
//		}
//	}

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
