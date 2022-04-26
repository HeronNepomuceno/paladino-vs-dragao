<template>
<div class="container">
	<header class="head">
		<h1 class="title">Derrote o Dragão!</h1>
	</header>
	<main class="body">
		<div class="bar-lifes">
			<div class="duelist">
				<span>Paladino</span>
				<div class="border-bar">
					<div class="bar1" style="background: green;" v-bind:style="{width: width1}"></div>
				</div>
				<label v-if="!paladinWin">{{ width1 }}</label>
				<label v-if="paladinWin">Você derrotou o dragão!</label>
			</div>
			<div class="duelist">
				<span>Dragão</span>
				<div class="border-bar">
					<div class="bar2" style="background: red;" v-bind:style="{width: width2}"></div>
				</div>
				<label v-if="!dragonWin">{{ width2 }}</label>
				<label v-if="dragonWin">O dragão derrotou você!</label>
			</div>
		</div>
		<div class="actions">
			<button class="btn btn1" v-on:click="confronto">Confronto</button>
			<button class="btn btn2" v-on:click="ataqueEspecial">Ataque Especial</button>
			<button class="btn btn3" v-on:click="porcaoDeCura">Porção de Cura</button>
			<button class="btn4" v-on:click="seRender">Se Render</button>
		</div>
		<ul class="memory">
		</ul>
	</main>
	<div class="snackbar" v-on:click="reloadGame">
		<h3>Para reiniciar o jogo, clique nesse cartaz!</h3>
	</div>
</div>
</template>

<script>
export default {
	name: 'TheGame',

	data: function () {
    return {
			width1: '',
			width2: '',
			valor1: 0,
			valor2: 0,
			paladinWin: false,
			dragonWin: false,
			countSpecial: 0,
			countHealPot: 0
    }
  },
	mounted() {
		this.valor1 = 100
		this.valor2 = 100
		this.width1 = `${this.valor1}%`
		this.width2 = `${this.valor2}%`
	},

	methods: {
		confronto() {
			let attackPaladin = parseInt(Math.random() * (13 - 7) + 7)
			let attackDragon = parseInt(Math.random() * (16 - 8) + 8)
			this.valor1 = this.valor1 - attackDragon
			this.width1 = `${this.valor1}%`
			this.valor2 = this.valor2 - attackPaladin
			this.width2 = `${this.valor2}%`

			if( this.valor1 <= 0 || this.valor2 <= 0) {
				for (let btn of document.querySelectorAll('.btn')) {
					btn.disabled = true
				}
			}

			let memory = document.querySelector('.memory')
			memory.insertAdjacentHTML('afterbegin', `<li style="background: #f9dedc; color: green; border: 1px solid #8c1d18; padding: 16px; font-size: 20px; text-align: center;"> O Paladino causou ${attackPaladin} pontos de dano!</li>`)
			memory.insertAdjacentHTML('afterbegin', `<li style="background: #f9dedc; color: red; border: 1px solid #8c1d18; padding: 16px; font-size: 20px; text-align: center;"> O Dragão causou ${attackDragon} pontos de dano!</li>`)
		},

		ataqueEspecial() {
			let attackSpecialPaladin = parseInt(Math.random() * (20 - 8) + 8)
			let attackDragon = parseInt(Math.random() * (16 - 8) + 8)
			this.valor1 = this.valor1 - attackDragon
			this.width1 = `${this.valor1}%`
			this.valor2 = this.valor2 - attackSpecialPaladin
			this.width2 = `${this.valor2}%`
			this.countSpecial = this.countSpecial + 1
			if (this.countSpecial == 3) {
				document.querySelector('.btn2').disabled = true
			}
			else if ( this.valor1 <= 0 || this.valor2 <= 0) {
				for (let btn of document.querySelectorAll('.btn')) {
					btn.disabled = true
				}
			}

			let memory = document.querySelector('.memory')
			memory.insertAdjacentHTML('afterbegin', `<li style="background: #f9dedc; color: green; border: 1px solid #8c1d18; padding: 16px; font-size: 20px; text-align: center;"> O Paladino causou ${attackSpecialPaladin} pontos de dano com seu ataque especial!</li>`)
			memory.insertAdjacentHTML('afterbegin', `<li style="background: #f9dedc; color: red; border: 1px solid #8c1d18; padding: 16px; font-size: 20px; text-align: center;"> O Dragão causou ${attackDragon} pontos de dano!</li>`)
		},

		porcaoDeCura() {
			let healPaladin = parseInt(Math.random() * (17 - 9) + 9)
			let attackDragon = parseInt(Math.random() * (16 - 8) + 8)

			this.valor1 = this.valor1 - attackDragon
			this.width1 = `${this.valor1}%`
			this.valor1 = this.valor1 + healPaladin
			this.width1 = `${this.valor1}%`

			this.countHealPot = this.countHealPot + 1
			if (this.countHealPot == 5) {
				document.querySelector('.btn3').disabled = true
			}
			else if ( this.valor1 <= 0 || this.valor2 <= 0) {
				for (let btn of document.querySelectorAll('.btn')) {
					btn.disabled = true
				}
			}

			let memory = document.querySelector('.memory')
			memory.insertAdjacentHTML('afterbegin', `<li style="background: #f9dedc; color: green; border: 1px solid #8c1d18; padding: 16px; font-size: 20px; text-align: center;"> O Paladino curou ${healPaladin} pontos de vida!</li>`)
			memory.insertAdjacentHTML('afterbegin', `<li style="background: #f9dedc; color: red; border: 1px solid #8c1d18; padding: 16px; font-size: 20px; text-align: center;"> O Dragão causou ${attackDragon} pontos de dano!</li>`)
		},

		seRender() {
			this.valor1 = 0
			this.width1 = `${this.valor1}%`
			document.querySelector('.snackbar').classList.add('show-snackbar')
		},

		reloadGame() {
			this.valor1 = 100
			this.valor2 = 100
			this.width1 = `${this.valor1}%`
			this.width2 = `${this.valor2}%`
			for (let btn of document.querySelectorAll('.btn')) {
				btn.disabled = false
			}
			this.countSpecial = 0
			this.countHealPot = 0
			this.paladinWin = false
			this.dragonWin = false
			document.querySelector('.snackbar').classList.remove('show-snackbar')
			document.querySelector('.memory').innerHTML = ''
		}
	},
	watch: {
		width1() {
			if (this.valor1 <= 0) {
				this.width1 = 0
				this.dragonWin = true

				document.querySelector('.snackbar').classList.add('show-snackbar')
			}
		},

		width2() {
			if(this.valor2 <= 0) {
				this.width2 = 0
				this.paladinWin = true

				document.querySelector('.snackbar').classList.add('show-snackbar')
			}
		}
	}
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.container {
	width: 100%;
	height: 100%;
	display: flex;
	flex-direction: column;
}

.head {
	display: flex;
	justify-content: center;
	align-items: center;
	margin-top: 16px
}

.title {
	font-family: var(--font);
	background: #f9dedc;
	padding: 16px;
	border-radius: 5px;
	border: solid 2px #8c1d18;
	color: #8c1d18;
}

.body {
	display: flex;
	justify-content: center;
	align-items: center;
	width: 100%;
	margin-top: 32px;
	flex-direction: column;
}

.bar-lifes {
	display: flex;
	justify-content: center;
	align-items: center;
	width: 100%;
}

.duelist {
	display: flex;
	justify-content: center;
	align-items: center;
	flex-direction: column;
	margin: 0 32px;
	background: #f9dedc;
	border: solid 2px #8c1d18;
	color: #8c1d18;
	padding: 32px;
	width: 30%;
}

.duelist span {
	font-family: var(--font);
	font-size: 26px;
	margin-bottom: 8px
}

.border-bar {
	border: solid 2px #8c1d18;
	height: 32px;
	width: 100%;
	display: flex;
	justify-content: flex-start;
	align-items: center;
	padding: 4px;
}

.bar1 {
	height: 28px;
}

.bar2 {
	height: 28px;
}

.actions {
	display: flex;
	width: 50%;
	justify-content: space-between;
	align-items: center;
	background: #f9dedc;
	border: solid 2px #8c1d18;
	margin-top: 16px;
	padding: 16px;
}

.btn {
	border: none;
	background: none;
	padding: 8px 16px;
	font-size: 20px;
	font-family: var(--font);
	color: #fff;
	border-radius: 15px;
	cursor: pointer;
}

.btn1 {
	background: #b3261e;
}
.btn2 {
	background: orange;
}
.btn3 {
	background: green;
}
.btn4 {
	border: none;
	background: none;
	padding: 8px 16px;
	font-size: 20px;
	font-family: var(--font);
	color: #fff;
	border-radius: 15px;
	cursor: pointer;
	background: grey;
}

.memory {
	display: flex;
	flex-direction: column;
	margin-top: 32px;
	width: 40%;
}

.snackbar{
	position: fixed;
	bottom: 32px;
	left: -1000px;
	background: #fff;
	color: #8c1d18;
	font-size: 18px;
	padding: 16px;
	border-radius: 5px;
	box-shadow: rgb(0 0 0 / 15%) 1.95px 1.95px 2.6px;
	transition: left 1s;
	cursor: pointer;
	transition: left .5s;
}

.show-snackbar {
	left: 32px;
}
</style>
