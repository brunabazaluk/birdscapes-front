<template>

	<div>
	<h1> BIRDSCAPES</h1>
	</div>

	<fieldset>  
		<legend>Horários</legend>

		<input v-model="time" type="radio" value="6:00">
		<label >amanhecer</label>

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
		<label >primavera</label>

		<input v-model="season" type="radio" id="notify-on" name="notify" value="verao">
		<label >verão</label>

		<input v-model="season" type="radio" id="notify-on" name="notify" value="outono">
		<label>outono</label>

		<input v-model="season" type="radio" id="notify-on" name="notify" value="inverno">
		<label>inverno</label>
	</fieldset>
	
	<p>{{ time }}</p>

</template>




<script>


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
				audio_passaro: ''
			}
		}
	}

</script>



<style>
</style>
