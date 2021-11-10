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

	var time;
	var season;
	var position;

	export default { 
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
		mounted() {
			fetch('http://127.0.0.1:5000/success')
			.then(res => res.json())
			.then(data => this.tudo = JSON.parse(data))
			.catch(err => console.log(err.message))
		}
	}

	var obj = new Object();
	obj.time = time;
	obj.season  = season;
	obj.position = position;
	//var json_obj= JSON.stringify(obj);




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
