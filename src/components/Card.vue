<script setup>

    defineProps({
        image: String,
        state: Boolean
    })
    
</script>

<template>

<div class="card" :class="{ open: state }" @click="$emit('onCardClick')">
	<div class="card-inner">
		<div class="card-front"></div>
		<div class="card-back">
			<img class="card-image" :src="image" alt="">
		</div>
	</div>
</div>

</template>

<style scoped>

.card,
.card-inner {
	position: relative;
	width: 100%;
	height: 100%;
}

.card {
	perspective: 1000px;
    aspect-ratio: 1 / 1;
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