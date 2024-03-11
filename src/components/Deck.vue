<script setup>

import { reactive, ref } from 'vue'
import Card from '@/components/Card.vue'

const emit = defineEmits(['allCardOpen', 'countEvent'])

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
				emit('countEvent')
			}
			clickedCard.length = 0
			clickDisable = false
			isWin()
		}, 2000)
	}
}


const isWin = () => {
	if (deck.every((elem) => elem.state == true)) {
		deck.forEach((card) => card.state = false)
		console.log('win');
		emit('allCardOpen', true)
	}
}

</script>

<template>

<div class="deck">
	<Card
		v-for="(card, index) in deck" :key="index"
		:image="card.image"
		:state="card.state"
		@onCardClick = "!card.state && clickDisable == false ? clickCard(index) : null"
	/>
</div>

</template>

<style lang="scss" scoped>

.deck {
	display: grid;
	grid-template-columns: repeat(4, 1fr);
	gap: 15px;

	@media (max-width: 575px) {
		grid-template-columns: repeat(3, 1fr);
		gap: 10px;
	}
}

</style>