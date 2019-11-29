<template>
	<div>
		
		<div class="container">
			<div class="menu">
				<input type="file" @change="saveFileContents($event)">
				<button @click="parseTextIntoParagraphs()">Параграфы</button>
				<button @click="save()">Save</button>
			</div>
			<hr>
			<div class="card-list">
				<div class="card" v-for="(p, index) in covey.paragraphs">
					<div class="card__content"> {{ p }} </div>
					<div class="card__index"> {{ index }} </div>
				</div>
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
	@import '../smart-grid.styl'
	button
		margin-right 5px	
	.container
		wrapper()
		max-width 540px
		// background-color #fff

	.menu
		margin-bottom 20px

	.card-list
		row-flex()

	.card
		col()
		position relative
		width 500px
		&__content
			padding 12px 
			border-radius 4px
			transition background-color 0.5s linear
			&:hover
				background-color #eadba7
		&__index
			// display none
			position absolute
			right: 5px
			bottom 5px
			font-size 0.7em
			color #999999 
				
</style>