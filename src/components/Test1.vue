<template>
	<div>
		<div>
			<input type="file" @change="saveOriginFileContents($event)">
			<button @click="parseTextIntoParagraphs()">Параграфы</button>
			<button @click="save()">Save</button>
		</div>
		<div>
			{{covey.origin}}
		</div>
	</div>
</template>

<script>
	export default {
		data(){
			return{
				covey: {},
			}
		},
		mounted(){
			let localCovey = localStorage.getItem('localCovey')
			this.covey = JSON.parse(localCovey) || {}
		},
		methods:{
			saveOriginFileContents(event){
				let reader = new FileReader();
				reader.onload =(event)=> {
					this.covey.origin = event.target.result;
				};
				reader.onerror = function(event) {
					console.error("Файл не может быть прочитан! код " + event.target.error.code);
				};
				reader.readAsText(event.target.files[0]);
			},
			parseTextIntoParagraphs(){
				// let re = /[А-ЯA-Z]((т.п.|т.д.|пр.)|[^?!.\(]|\([^\)]*\))*[.?!]/g
				// let sentences = this.original.match(re)
				let re = /\n/g
				this.covey.paragraphs = this.covey.original.split(re)
			},
			save(){
				localStorage.setItem('localCovey', JSON.stringify(this.covey))
			},
		}
	}
</script>

<style lang="stylus">
	button
		margin-right 5px	
</style>