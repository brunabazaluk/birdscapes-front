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
		<p>{{ tudo }}</p>	
	</div>

</template>




<script>
	import axios from 'axios'

	export default {
		created() {	 
			this.getData();
			//this.fetchData();
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
			getData() {
				axios({
					baseURL: 'https://birdscapes.herokuapp.com',
					url: '/success',
					method: 'post',
					data: {
						season: 'inverno',
						time: '11:00',
						position: '200'
					}
				}).then(res=>{
							console.log(res);
				}).catch(err=>{
						console.log(err);
				});

				fetch('https://birdscapes.herokuapp.com/success')
				.then(res => res.json())
				.then(data => this.tudo = data)
				.catch(err => console.log(err.message))
			
			},

		},
//		mounted() {
//			//fetch('http://127.0.0.1:5000/success')
//			fetch('https://birdscapes.herokuapp.com/success')
//			.then(res => res.json())
//			.then(data => this.tudo = data)
//			.catch(err => console.log(err.message))
//		}

	}

//	export default { 
//		data() {
//			return {
//				time: '10:00',
//				season: 'primavera',
//				position: '100',
//				imgs: [],
//				popular: [],
//				latim: [],
//				ingles: [],
//				audio_passaro: '',
//				tudo: []
//			}
//		},
//		mounted() {
//			//fetch('http://127.0.0.1:5000/success')
//			fetch('https://birdscapes.herokuapp.com/success')
//			.then(res => res.json())
//			.then(data => this.tudo = data)
//			.catch(err => console.log(err.message))
//		}
//	}
//

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
