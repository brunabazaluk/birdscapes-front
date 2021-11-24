<template>


	<td  v-for="(img,i) in imgs" :key="i">
		<img width= "300" :src="'https://birdscapes.herokuapp.com/image/' + img"  > 
		<p class="pop" >{{ popular[i] }}</p>
		<p class="ing" >{{ ingles[i] }}</p>
		<p class="lat" >{{ latim[i] }}</p>
	</td>


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
		playVozes(vozes){
			let sound = []
			let n = vozes.length
			for (let i = 0; i<n; i++){
				sound[i] = new Howl({
					src: 'https://birdscapes.herokuapp.com/voz/' + vozes[i],
					format: ['mp3', 'aac'],
					volume: 1 - 0.2*i,
					loop: true,
				});
				sound[i].play();
			}
		}
		
	},

	watch: {
		season: function() { this.getData() },
		time: function() { this.getData() },
		vozes: function() { this.playVozes(this.vozes) }
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

	.pop {
		font-size: 14pt;
		font-weight: bold;
	}
	.ing {
		font-size: 12pt;
	}
	.lat {
		font-size: 12pt;
		font-style: italic;
	}
</style>
