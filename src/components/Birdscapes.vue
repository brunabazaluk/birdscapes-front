<template>

	<img class="header" alt="Birdscapes logo" src="../assets/birdscapes.png">
	
	<div class="quadro">
		<td v-for="(img,i) in imgs" :key="i">
			<a><img class="birds" :width="img_wid[i]" :src="'https://birdscapes.herokuapp.com/image/' + img"></a>
			<p class="names">
				<span class="pop" >{{ popular[i] }}</span><br>
				<span class="ing" >{{ ingles[i] }}</span><br>
				<span class="lat" >{{ latim[i] }}</span>
			</p>
		</td>
	</div>

	<div>
		<button type="button" v-on:click="getData">Iniciar</button>
	</div>

	<div>
		<p> Volume Geral</p>
		<input v-model="vol" type="range" min=0 max=1 step="0.01"> 
	</div>


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
import {Howl, Howler} from 'howler';

const $axios = axios.create({
	baseURL: 'https://birdscapes.herokuapp.com'
});

export default {
	mounted() {	 
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
			vol: 1,
			back_vol: 1,
			mata_vol: 1,
			chuva_vol: 1,
			sound: [],
			img_wid: [250,225,200,175,150,125,100,75],
			img_x: [],
			img_y: [],
		}
	},

	methods: {
		async getData() {
			const { data } = await $axios.post('/', {
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
		playVozes(vozes,sound){
			let n = vozes.length
			Howler.stop()
			sound = sound.splice(0)
			for (let i = 0; i<n; i++){
				let stereo = this.randomPos();
				sound[i] = new Howl({
					src: 'https://birdscapes.herokuapp.com/voz/' + vozes[i],
					format: ['mp3', 'aac'],
					volume: (1 - 0.2*i)*this.vol,
					loop: false,
					stereo: stereo,
					onend: function() {
						setTimeout(function() {sound[i].progress = 0;sound[i].play()}, 5000)
					},
				});
				sound[i].play();
				this.img_x[i] = 100*(i+1)*stereo;
				this.img_y[i] = 100*Math.sqrt( ((i+1)**2) - (this.img_x[i])**2 );
			
			}
			console.warn(sound);
		},
		sonsBackground(){
			let chuva = new Howl({
				src: 'https://birdscapes.herokuapp.com/voz/chuva_com_sapos.mp3',
				format: ['mp3', 'aac'],
				volume: this.chuva_vol*this.vol,
				loop: true,
			});
			let mata = new Howl({
				src: 'https://birdscapes.herokuapp.com/voz/mata.mp3',
				format: ['mp3', 'aac'],
				volume: this.mata_vol*this.vol,
				loop: true,
			});
			chuva.play();
			mata.play();
			if (this.time == '10:00' || this.time == '6:00' ){
				let back = new Howl({
					src: 'https://birdscapes.herokuapp.com/voz/vento_diurno.mp3',
					format: ['mp3', 'aac'],
					volume: this.back_vol*this.vol,
					loop: true,
				});
				back.play();
			}
			else if (this.time == '16:00'){
				let back = new Howl({
					src: 'https://birdscapes.herokuapp.com/voz/tarde_com_grilos.mp3',
					format: ['mp3', 'aac'],
					volume: this.back_vol*this.vol,
					loop: true,
				});
				back.play();
			}
			else if (this.time == '19:00'){
				let back = new Howl({
					src: 'https://birdscapes.herokuapp.com/voz/noturna_com_saponho.mp3',
					format: ['mp3', 'aac'],
					volume: this.back_vol*this.vol,
					loop: true,
				});
				back.play();
			}
		
		},
		setVol(sound,vol){
			console.warn(sound);
			Howler.volume(vol);
		},
		setBackVol(back, vol){
			back.volume(vol);
		},
		randomPos(){
			let n = Math.random();
			return n*2 - 1;
		},
		
	},

	watch: {
		season: function() { this.getData() },
		time: function() { this.getData() },
		vozes: function() { this.playVozes(this.vozes, this.sound) },
		vol: function() { this.setVol(this.sound, this.vol) },
		//mata_vol: function() { this.setBackVol(this.mata, this.mata_vol*this.vol) },
		//chuva_vol: function() { this.setBackVol(this.chuva, this.chuva_vol*this.vol) },
		//back_vol: function() { this.setBackVol(this.back, this.back_vol*this.vol) },
	},

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

	img.header {
		width: 400px;
		height: auto;
		top: 20%;
	}

	.pop {
		font-size: 12pt;
		font-weight: bold;
	}
	.ing {
		font-size: 10pt;
	}
	.lat {
		font-size: 10pt;
		font-style: italic;
	}
	p.names {
		display: none;
		height:30px;
		margin-left:10px;
	}

	a:hover + p.names {
		display: block;
	}

	.birds {
		position: relative;
	}

	.quadro {
		min-height: 6cm;
	}
</style>
