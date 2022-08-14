<script setup>
import { reactive, ref } from "vue";

const coffees = reactive([
	{ name: "Expresso", coffee: 80, bubble: 0, water: 0, milk: 0, type: "small" },
	{
		name: "Americano",
		coffee: 40,
		bubble: 0,
		water: 40,
		milk: 0,
		type: "medium",
	},
	{
		name: "Cappuccino",
		coffee: 30,
		bubble: 0,
		water: 0,
		milk: 50,
		type: "large",
	},
	{ name: "Latte", coffee: 30, bubble: 30, water: 0, milk: 30, type: "mock" },
]);
const cupTypes = ref(["small", "medium", "large", "mock"]);
const favoriteType = ref("Cappuccino");
const editing = ref(true);
</script>

<template>
	<div class="editorButton">
		<label for="editorButton"> Editing: </label>
		<input type="checkbox" id="editorButton" v-model="editing" />
		<select id="" v-model="favoriteType">
			<option v-for="coffee in coffees" :value="coffee.name">
				{{ coffee.name }}
			</option>
		</select>
	</div>
	<h1>Coffee Editor</h1>
	<div
		class="coffeeType"
		:class="{ favorite: coffee.name === favoriteType, showMode: !editing }"
		v-for="(coffee, index) in coffees"
		:key="coffee.name"
	>
		<div class="num">0{{ index }}</div>
		<div class="cupContainer">
			<div class="cup" :class="[coffee.type]">
				<div class="ingradients">
					<div class="coffee" :style="{ height: coffee.coffee + '%' }"></div>
					<div class="milk" :style="{ height: coffee.milk + '%' }"></div>
					<div class="water" :style="{ height: coffee.water + '%' }"></div>
					<div class="bubble" :style="{ height: coffee.bubble + '%' }"></div>
				</div>
			</div>
		</div>
		<div class="control">
			<h2>
				{{ coffee.name }}
			</h2>
			<select v-model="coffee.type">
				<option :value="cupType" v-for="cupType in cupTypes">
					{{ cupType }}
				</option>
			</select>
			<div v-for="type in ['coffee', 'milk', 'water', 'bubble']">
				<label for="bar">{{ type }}</label>
				<input
					type="range"
					name="bar"
					v-model="coffee[type]"
					min="0"
					max="100"
				/>
			</div>
		</div>
	</div>
</template>

<style lang="scss" scoped>
@mixin size($width, $height: $width) {
	width: $width;
	height: $height;
}
h1 {
	text-align: center;
	padding-top: 2rem;
}
.coffeeType {
	display: grid;
	align-items: center;
	min-width: clamp(20rem, 90vw, 25rem);
	@media (min-width: 40em) {
		grid-template-columns: 1fr 1fr 2fr;
		min-width: 30rem;
	}
	border-radius: 5px;
	padding: 10px 20px;
	border-bottom: solid 1px rgb(255 255 255 / 0.1);
	transition: 0.5s;
	&:hover {
		background-color: rgb(255 255 255 / 0.05);
	}
	.num {
		font-size: 60px;
		@media (max-width: 40em) {
			font-size: 2.4rem;
			grid-row: 1 / 2;
			margin: 0 auto;
		}
		font-weight: bold;
		margin-right: 30px;
		padding-top: 5px;
	}
	.cupContainer {
		@media (max-width: 40em) {
			// grid-column: 1 / 3;
			grid-row: 2 / 3;
		}
	}
	.control {
		padding-left: 2.4rem;
		display: grid;
		gap: 0.5rem;
		@media (max-width: 40em) {
			justify-content: center;
			// display: block;
			text-align: center;
			padding-left: 0;
		}

		select {
			width: 81%;
			@media (max-width: 40em) {
				margin: 0 auto 1rem;
			}
		}
		div {
			display: flex;
			justify-content: space-between;
			@media (max-width: 40em) {
				width: 18rem;
				width: clamp(14rem, 60vw, 18rem);
			}
		}
	}
	.cup {
		border: solid 4px #fff;
		@include size(70px, 50px);
		border-top: none;
		border-radius: 0 0 50px 50px;
		margin: 10px auto;
		position: relative;
		&::before {
			content: "";
			display: block;
			border: 4px solid #fff;
			border-left: none;
			@include size(1rem, 24px);
			border-radius: 0 50px 50px 0;
			position: absolute;
			left: 100%;
			top: 40%;
			transform: translateY(-50%);
		}
		.ingradients {
			@include size(100%);
			border-radius: 0 0 50px 50px;
			overflow: hidden;
			display: flex;
			flex-direction: column-reverse;
			padding-top: 2.4px;
			.coffee {
				background-color: hsl(15, 29%, 44%);
			}
			.milk {
				background-color: #fff;
			}
			.water {
				background-color: rgb(158, 207, 255);
			}
			.bubble {
				background-image: url(https://media.istockphoto.com/photos/closeup-abstract-caramel-shapes-picture-id1212894836?k=20&m=1212894836&s=612x612&w=0&h=PyQJyOsHZTbgRKwGz1K870t6g0vVKJDQnRzrmOHXKuw=);
				background-size: cover;
			}
		}

		&.small {
			@include size(50px, 40px);
		}
		&.large {
			@include size(81px, 50px);
		}
		&.mock {
			@include size(60px, 90px);
			border-radius: 0;
			.ingradients {
				border-radius: 0;
			}
		}
	}
	&.favorite {
		&::before {
			content: "BEST";
			color: #ff5e5e;
			border: 4px solid;
			padding: 5px 3px;
			line-height: 0.81;
			font-size: 30px;
			border-width: 5px;
			position: absolute;
			transform: translateY(-8rem) rotate(-24deg);
			@media (min-width: 40em) {
				transform: translateX(-70px) rotate(-24deg);
			}
		}
	}
	&.showMode {
		label,
		input,
		select {
			display: none;
		}
		h2 {
			margin-bottom: -1rem;
		}
		&.favorite {
			&::before {
				transform: translateY(-3rem) rotate(-24deg);
			}
		}
	}
}
.editorButton {
	position: fixed;
	left: 0.8rem;
	top: 0.8rem;
	select {
		margin-left: 2rem;
	}
}
</style>
