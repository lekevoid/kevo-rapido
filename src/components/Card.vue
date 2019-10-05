<template>
	<div class="card_zone">
		<div :class="['card', color]">
			<div class="icon">
				<img :src="icon" />
			</div>
			<div class="letter">{{letter}}</div>
		</div>
		<PlayerBoard :showScores="showScores" />
	</div>
</template>

<script>
	import PlayerBoard from "./PlayerBoard.vue";

	import animal_blue from "../img/animal_blue.png";
	import animal_green from "../img/animal_green.png";
	import animal_orange from "../img/animal_orange.png";
	import celebrity_blue from "../img/celebrity_blue.png";
	import celebrity_green from "../img/celebrity_green.png";
	import celebrity_orange from "../img/celebrity_orange.png";
	import city_blue from "../img/city_blue.png";
	import city_green from "../img/city_green.png";
	import city_orange from "../img/city_orange.png";
	import country_blue from "../img/country_blue.png";
	import country_green from "../img/country_green.png";
	import country_orange from "../img/country_orange.png";
	import cover from "../img/cover.png";
	import food_blue from "../img/food_blue.png";
	import food_green from "../img/food_green.png";
	import food_orange from "../img/food_orange.png";
	import movie_blue from "../img/movie_blue.png";
	import movie_green from "../img/movie_green.png";
	import movie_orange from "../img/movie_orange.png";
	import name_blue from "../img/name_blue.png";
	import name_green from "../img/name_green.png";
	import name_orange from "../img/name_orange.png";
	import object_blue from "../img/object_blue.png";
	import object_green from "../img/object_green.png";
	import object_orange from "../img/object_orange.png";
	import plant_blue from "../img/plant_blue.png";
	import plant_green from "../img/plant_green.png";
	import plant_orange from "../img/plant_orange.png";
	import profession_blue from "../img/profession_blue.png";
	import profession_green from "../img/profession_green.png";
	import profession_orange from "../img/profession_orange.png";

	export default {
		name: "Card",
		components: {
			PlayerBoard
		},
		props: {
			type: String,
			letter: String,
			color: String,
			showScores: Boolean
		},
		computed: {
			icon() {
				const allIcons = {
					"animal_blue" : animal_blue,
					"animal_green" : animal_green,
					"animal_orange" : animal_orange,
					"celebrity_blue" : celebrity_blue,
					"celebrity_green" : celebrity_green,
					"celebrity_orange" : celebrity_orange,
					"city_blue" : city_blue,
					"city_green" : city_green,
					"city_orange" : city_orange,
					"country_blue" : country_blue,
					"country_green" : country_green,
					"country_orange" : country_orange,
					"cover" : cover,
					"food_blue" : food_blue,
					"food_green" : food_green,
					"food_orange" : food_orange,
					"movie_blue" : movie_blue,
					"movie_green" : movie_green,
					"movie_orange" : movie_orange,
					"name_blue" : name_blue,
					"name_green" : name_green,
					"name_orange" : name_orange,
					"object_blue" : object_blue,
					"object_green" : object_green,
					"object_orange" : object_orange,
					"plant_blue" : plant_blue,
					"plant_green" : plant_green,
					"plant_orange" : plant_orange,
					"profession_blue" : profession_blue,
					"profession_green" : profession_green,
					"profession_orange" : profession_orange
				};

				return allIcons[this.type.toLowerCase() + "_" + this.color.toLowerCase()];
			}
		}
	}
</script>