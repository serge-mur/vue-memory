<script setup>

import { reactive, ref, watch } from 'vue'

const preDeck = [
	{ name: '1', image: '../src/assets/icons/apple-svgrepo-com.svg', state: false },
	{ name: '2', image: '../src/assets/icons/beer-svgrepo-com.svg', state: false },
	{ name: '3', image: '../src/assets/icons/pizza-svgrepo-com.svg', state: false },
	{ name: '4', image: '../src/assets/icons/beef-svgrepo-com.svg', state: false },
	{ name: '5', image: '../src/assets/icons/hot-dog-svgrepo-com.svg', state: false },
	{ name: '6', image: '../src/assets/icons/hawthorn-svgrepo-com.svg', state: false },

]

const deck = reactive(preDeck.concat(JSON.parse(JSON.stringify(preDeck))).sort(() => 0.5 - Math.random()))

const clickedCard = reactive([])

const isWin = ref(false)

let clickDisable = false

const clickCard = (index) => {
	deck[index].state = true
	clickedCard.push(deck[index])
	if (clickedCard.length == 2) {
		clickDisable = true
		setTimeout(() => {
			if (clickedCard[0].name !== clickedCard[1].name) {
				clickedCard[0].state = false
				clickedCard[1].state = false
			}
			clickedCard.length = 0
			isWin.value = deck.every((elem) => elem.state == true)
			clickDisable = false
		}, 2000)
	}
}

watch(isWin, () => {
	deck.forEach((card) => card.state = false)
})

</script>

<template>
	<div class="wrapper">
		<h1 class="title">Memory Game</h1>
		<div class="deck" v-if="!isWin">
			<div class="card" :class="{ open: card.state }" v-for="(card, index) in deck" :key="index"
				@click="!card.state && clickDisable == false ? clickCard(index) : null">

				<div class="card-inner">
					<div class="card-front"></div>
					<div class="card-back">
						<img class="card-image" :src="card.image" alt="">
					</div>
				</div>

			</div>
		</div>
		<div v-else>
			<div class="message">
				<h3>You Win!</h3>
				<button type="button" @click="isWin = false">Try again?</button>
			</div>

		</div>

	</div>
</template>

<style lang="scss" scoped>
.wrapper {
	max-width: 800px;
	background-color: #eee;
	margin: 15px auto;
	padding: 15px;
	height: 100vh;

	.title {
		text-align: center;
	}

	.message {
		border: 1px solid #ccc;
		padding: 15px 15px 45px 15px;
		background-color: #fff;
		width: 250px;
		margin: 30px auto;
		text-align: center;

		button {
			&:hover {
				cursor: pointer;
			}
		}
	}
}

.deck {
	display: grid;
	grid-template-columns: repeat(4, 1fr);
	gap: 15px;

	@media (max-width: 575px) {
		grid-template-columns: repeat(3, 1fr);
		gap: 10px;
	}

	.card {
		aspect-ratio: 1 / 1;
	}
}

.card,
.card-inner {
	position: relative;
	width: 100%;
	height: 100%;
}

.card {
	perspective: 1000px;

	&:hover {
		cursor: pointer;
	}

	&.open {
		.card-inner {
			transform: rotateY(180deg);
		}
	}

	.card-image {
		max-width: 100%;
	}
}

.card-inner {
	transition: transform .5s ease-in-out;
	transform-origin: 50% 50%;
	transform-style: preserve-3d;
}

.card-back {
	transform: rotateY(180deg);
}

.card-front,
.card-back {
	position: absolute;
	top: 0;
	left: 0;
	display: flex;
	width: 100%;
	height: 100%;
	background-color: #fff;
	border: 1px solid #ccc;
	justify-content: center;
	align-items: center;
	backface-visibility: hidden;
}

.card-front {
	background-image: linear-gradient(45deg, rgba(0, 0, 0, 0) 48%, rgba(255, 255, 255, 0.3) 50%, rgba(0, 0, 0, 0) 52%), linear-gradient(-45deg, rgba(0, 0, 0, 0) 48%, rgba(255, 255, 255, 0.3) 50%, rgba(0, 0, 0, 0) 52%);
	background-size: 1em 1em;
	background-color: #ccc;
}
</style>
