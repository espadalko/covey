<template>
	<div>
		<div class="menu">
			<input type="file" @change="saveFileContents($event)">
			<button @click="parseTextIntoParagraphs()">Параграфы</button>
			<button @click="save()">Save</button>
		</div>
		<div class="card-list">
			<div class="card" v-for="(p, index) in covey.paragraphs">
				<div class="card__content"> {{ p }} </div>
				<div class="card__index"> {{ index }} </div>
				<hr class="card__hr">
			</div>
		</div>
	</div>
</template>

<script>
	export default {
		data(){
			return{
				covey: {
					origin: '',
					paragraphs: []
				},
			}
		},
		mounted(){
			let localCovey = localStorage.getItem('localCovey')
			this.covey = localCovey ? JSON.parse(localCovey) : this.covey   
		},
		methods:{
			saveFileContents(event){
				let reader = new FileReader();
				reader.onload = (event)=> {
					this.covey.origin = event.target.result;
					console.log(this.covey)
				};
				reader.onerror = function(event) {
					console.error("Файл не может быть прочитан! код " + event.target.error.code);
				};
				reader.readAsText(event.target.files[0]);
			},
			// Text
			parseTextIntoParagraphs(){
				// let re = /[А-ЯA-Z]((т.п.|т.д.|пр.)|[^?!.\(]|\([^\)]*\))*[.?!]/g
				// let sentences = this.original.match(re)
				let re = /\n/g
				let paragraphs = this.covey.origin.split(re)
				paragraphs = paragraphs.filter(element => element !== '')
				this.covey.paragraphs = paragraphs
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

	.menu
		margin-bottom 20px
	.card-list
		display flex
		flex-direction column

	.card
		position relative
		width 500px
		// padding 10px 20px
		// margin 4px
		// background-color #fff
		transition background-color 0.5s linear
		&__content
			// background-color red
		&__hr
			// margin-top 5px
		&__index
			display none
			position absolute
			right: 5px
			bottom 5px
			font-size 0.7em
			color #999999 
		&:hover
			background-color #eadba7
			&--index
				display 
				
</style>