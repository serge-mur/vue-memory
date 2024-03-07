<script setup>

import { ref } from 'vue'
import Deck from '@/components/Deck.vue'

const isWin = ref(false)

</script>

<template>
	<div class="wrapper">
		<h1 class="title">Memory Game</h1>
		<div v-if="!isWin">

			<Deck @allCardOpen="(val) => isWin = val" />

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

</style>
