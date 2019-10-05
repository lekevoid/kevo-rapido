<template>
	<div id="app" :class="[{ is_running: gameStarted }, 'players_'+playersNum]">
		<div id="toolbar">
			<div class="dash players">
				<button id="add_player" @click="changePlayersNum(1)"><img :src="icon_add_player" /></button>
				<button id="remove_player" @click="changePlayersNum(-1)"><img :src="icon_remove_player" /></button>
			</div>
			<div class="dash scores">
				<button id="toggle_scores" @click="toggleScores"><img :src="icon_scores" /></button>
			</div>
			<div class="dash labels">
				<button id="toggle_labels" @click="toggleLabels"><img :src="icon_labels" /></button>
			</div>
		</div>
		<div id="hull">
			<div id="turnCard">
				<button @click="newTurn" id="new_turn"></button>
			</div>
			<div id="cardsBoard">
				<Card v-for="i in playersNum" :key="i" v-if="gameStarted" :type="currentCard.type" :letter="currentCard.letter" :color="currentCard.color" :showScores="showScores" />
			</div>
		</div>
	</div>
</template>

<script>
	import Card from "./components/Card.vue";
	import "./styles/styles.css";

	import icon_add_player from "./img/icon_add_player.svg";
	import icon_remove_player from "./img/icon_remove_player.svg";
	import icon_scores from "./img/icon_scores.svg";
	import icon_labels from "./img/icon_labels.svg";

	export default {
		name: "app",

		components: {
			Card
		},

		data() {
			return {
				playersNum: 2,
				currentCard: {
					type: "",
					letter: "",
					color: "",
				},
				gameStarted: false,
				showScores: false,
				showLabels: false,
				colors: [
					"blue",
					"green",
					"orange"
				],
				types: [
					"Animal",
					"City",
					"Country",
					"Celebrity",
					"Profession",
					"Object",
					"Plant",
					"Food",
					"Name",
					"Movie",
				],
				lettersMR: "AAAAAAAAAABBBBBBBBCCCCCCCDDDDDDDDEEEEEEEFFFFFFFFGGGGGHHHHHHHIIIIIJJJJJJJKKKKKLLLLLLLLLMMMMMMMMMMNNNNNNNNNNOOOOOOPPPPPPPPQQQRRRRRRRRRRSSSSSSSSSSTTTTTTTTTTTTUUUUVVVVVWWWWWWXXXYYYYZZZ",
				lettersScrabbleFR: "AAAAAAAAABBCCDDDEEEEEEEEEEEEEEEFFGGHHIIIIIIIIJKLLLLLMMMNNNNNNOOOOOOPPQRRRRRRSSSSSSTTTTTTUUUUUUVVWXYZ",
				lettersScrabbleEN: "AAAAAAAAABBCCDDDDEEEEEEEEEEEEFFGGGHHIIIIIIIIIJKLLLLMMNNNNNNOOOOOOOOPPQRRRRRRSSSSTTTTTTUUUUVVWWXYYZ",
				lettersCities: "AAAAAAAAAAAAAAAAAAAAAAAAAAABBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBCCCCCCCCCCCCCCCCCCCCCCCCCCCCCDDDDDDDDDDDDDDDEEEEEEEFFFFFFFFGGGGGGGGGGGGGGGHHHHHHHHHHHHHHHHHHIIIIIIIIIIJJJJJJJJJJJKKKKKKKKKKKKKKKKKKKKKKKKKKKKKLLLLLLLLLLLLLLLLLLMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMNNNNNNNNNNNNNNNNNNOOOOOOOOOPPPPPPPPPPPPPPPPPPPPPPPQQQQRRRRRRRRRRRRRSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSTTTTTTTTTTTTTTTTTTTTTTTTTTUUUUUVVVVVVVVVWWWWWWXXXYYYYYYYZZZZZZ",
				lettersCountries: "AAAAAAAAAAABBBBBBBBBBBBBBBBBCCCCCCCCCCCCCCCCCDDDDDEEEEEEEEFFFGGGGGGGGGGGHHHHIIIIIIIIJJJKKKKKLLLLLLLLLMMMMMMMMMMMMMMMMMMNNNNNNNNNNNOPPPPPPPPPPQRRRSSSSSSSSSSSSSSSSSSSSSSSSSSTTTTTTTTTTTUUUUUUUVVVYZZ",
				lang: "fr",
				turn: 0,
				icon_add_player: icon_add_player,
				icon_remove_player: icon_remove_player,
				icon_scores: icon_scores,
				icon_labels: icon_labels
			}
		},

		computed: {
			randomKey() {
				return Math.ceil(Math.random() * 999999999);
			}
		},

		methods: {
			shuffle(obj) {
				if (typeof(obj) === "string") {
					obj = obj.split("");
				}
				for (let i=obj.length-1; i>0; i--) {
					const j = Math.floor(Math.random() * (i + 1));
					[obj[i], obj[j]] = [obj[j], obj[i]];
				}
				return obj;
			},

			newTurn() {
				this.turn++;
			},

			chooseType() {
				const shuffledTypes = this.shuffle(this.types);
				this.currentCard.type = shuffledTypes[Math.floor(Math.random() * shuffledTypes.length)];
				this.chooseLetter();
			},

			chooseLetter() {
				const currentType = this.currentCard.type;
				let compendium = [this.lettersMR, this.lettersMR];

				if (currentType === "Country") {
					compendium.push(this.lettersCountries);
				} else if (currentType === "City") {
					compendium.push(this.lettersCities);
				} else if (this.lang === "en") {
					compendium.push(this.lettersScrabbleEN);
				} else {
					compendium.push(this.lettersScrabbleFR);
				}

				const chosenCompendium = this.shuffle(compendium[Math.floor(Math.random() * compendium.length)]);
				this.currentCard.letter = chosenCompendium[Math.floor(Math.random() * chosenCompendium.length)];
			},

			chooseColor() {
				this.currentCard.color = this.colors[Math.floor(Math.random() * this.colors.length)];
			},

			changePlayersNum(val) {
				console.log(this.playersNum + val);
				if ((this.playersNum + val) >= 2 && (this.playersNum + val) <= 6) {
					this.playersNum += val;
				}
			},

			toggleScores() {
				this.showScores = !this.showScores;
			},

			toggleLabels() {
				this.showLabels = !this.showLabels;
			}
		},

		watch: {
			turn() {
				if (this.turn > 0) {
					this.gameStarted = true;
				}
				// chooseType() will take care of choosing the letter too.
				this.chooseType();
				this.chooseColor();
			},
		}
	}
</script>
