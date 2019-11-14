<template>
	<div id="global">
		<div id="app" :class="[{ is_running: gameStarted, single_card_mode: singleCard }, 'players_'+playersNum]">
			<div id="toolbar" :class="[{open: toolbarOpen}]">
				<button id="open" @click="toggleOpenToolbar"></button>
				<div class="dash players double">
					<button id="add_player" @click="changePlayersNum(1)"><img :src="icon_add_player" /></button>
					<button id="remove_player" @click="changePlayersNum(-1)"><img :src="icon_remove_player" /></button>
				</div>
				<div class="dash scores">
					<button id="toggle_scores" :class="[{ active: showScores }]" @click="toggleScores"><img :src="icon_scores" /></button>
				</div>
				<div class="dash labels">
					<button id="toggle_labels" :class="[{ active: showLabels }]" @click="toggleLabels"><img :src="icon_labels" /></button>
				</div>
				<div class="dash single_card">
					<button id="toggle_labels" :class="[{ active: singleCard && !disableSingleCardBtn }]" @click="toggleSingleCard" :disabled="disableSingleCardBtn">Single Card</button>
				</div>
				<div :class="['dash', 'random_rotate', { double: !cardRandomRotate }]">
					<button id="toggle_labels" :class="[{ active: cardRandomRotate }]" @click="toggleRandomRotate">Random Rotate</button>
					<button id="toggle_labels" :class="[{ hide: cardRandomRotate }]" @click="rotateCard90">Rotate 90&deg;</button>
				</div>
			</div>
			<div id="hull">
				<div id="turnCard">
					<button @click="newTurn" id="new_turn"></button>
				</div>
				<div id="cardsBoard">
					<PlayerBoard v-for="i in playersNum" :key="'board'+i" :playerNum="i" :gameStarted="gameStarted" :currentCard="currentCard" :handicap="playerHandicaps[i-1]" :showScores="showScores" :rotateOrigValue="playerBoardsRotates[i-1]" :turn="turn" />
					<div v-for="i in playersNum" :key="'options'+i" :class="['modal', { active : showPlayerOptions === i }]" @click="togglePlayerOptions(i)">
						<div class="options" @click.stop.prevent>
							<h2>Player {{i}} Options</h2>
							<table>
								<tr>
									<td>Handicap</td>
									<td>
										<button @click="changePlayerHandicap(i, -1)" :disabled="handicapChangeDisabled">
											<span class="btn_arrow left"></span>
										</button>
									</td>
									<td>
										<div class="handicap">{{ playerHandicaps[i-1] }}</div>
									</td>
									<td>
										<button @click="changePlayerHandicap(i, 1)" :disabled="handicapChangeDisabled">
											<span class="btn_arrow right"></span>
										</button>
									</td>
								</tr>
							</table>
						</div>
					</div>
					<Card id="the_one" :gameStarted="gameStarted" :currentCard="currentCard" :showScores="showScores" :singleCardRotation="singleCardRotation" :randomRotate="cardRandomRotate" :staticRotate="cardStaticRotate" />
				</div>
			</div>
		</div>
	</div>
</template>

<script>
	import Card from "./components/Card.vue";
	import PlayerBoard from "./components/PlayerBoard.vue";
	import "./styles/styles.css";

	import icon_add_player from "./img/icon_add_player.svg";
	import icon_remove_player from "./img/icon_remove_player.svg";
	import icon_scores from "./img/icon_scores.svg";
	import icon_labels from "./img/icon_labels.svg";

	export default {
		name: "app",

		components: {
			Card,
			PlayerBoard
		},

		data() {
			return {
				colors: [
					"blue",
					"green",
					"orange"
				],
				currentCard: {
					type: "",
					letter: "",
					color: "",
				},
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
					"Book",
					"Brand",
					"Event",
					"Fictional_Character",
					"Game",
					"Houseware",
					"Location",
					"Mythology",
					"Adjective",
					"Song",
					"Verb",
				],
				playersNum: 2,
				playerBoardsRotates: [
					0,
					180
				],
				playerHandicaps: [
					0,
					0
				],
				handicapChangeDisabled: false,
				gameStarted: false,
				showScores: false,
				showLabels: false,
				disableSingleCardBtn: false,
				lettersMR: "AAAAAAAAAABBBBBBBBCCCCCCCDDDDDDDDEEEEEEEFFFFFFFFGGGGGHHHHHHHIIIIIJJJJJJJKKKKKLLLLLLLLLMMMMMMMMMMNNNNNNNNNNOOOOOOPPPPPPPPQQQRRRRRRRRRRSSSSSSSSSSTTTTTTTTTTTTUUUUVVVVVWWWWWWXXXYYYYZZZ",
				lettersScrabbleFR: "AAAAAAAAABBCCDDDEEEEEEEEEEEEEEEFFGGHHIIIIIIIIJKLLLLLMMMNNNNNNOOOOOOPPQRRRRRRSSSSSSTTTTTTUUUUUUVVWXYZ",
				lettersScrabbleEN: "AAAAAAAAABBCCDDDDEEEEEEEEEEEEFFGGGHHIIIIIIIIIJKLLLLMMNNNNNNOOOOOOOOPPQRRRRRRSSSSTTTTTTUUUUVVWWXYYZ",
				lettersCities: "AAAAAAAAAAAAAAAAAAAAAAAAAAABBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBCCCCCCCCCCCCCCCCCCCCCCCCCCCCCDDDDDDDDDDDDDDDEEEEEEEFFFFFFFFGGGGGGGGGGGGGGGHHHHHHHHHHHHHHHHHHIIIIIIIIIIJJJJJJJJJJJKKKKKKKKKKKKKKKKKKKKKKKKKKKKKLLLLLLLLLLLLLLLLLLMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMMNNNNNNNNNNNNNNNNNNOOOOOOOOOPPPPPPPPPPPPPPPPPPPPPPPQQQQRRRRRRRRRRRRRSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSSTTTTTTTTTTTTTTTTTTTTTTTTTTUUUUUVVVVVVVVVWWWWWWXXXYYYYYYYZZZZZZ",
				lettersCountries: "AAAAAAAAAAABBBBBBBBBBBBBBBBBCCCCCCCCCCCCCCCCCDDDDDEEEEEEEEFFFGGGGGGGGGGGHHHHIIIIIIIIJJJKKKKKLLLLLLLLLMMMMMMMMMMMMMMMMMMNNNNNNNNNNNOPPPPPPPPPPQRRRSSSSSSSSSSSSSSSSSSSSSSSSSSTTTTTTTTTTTUUUUUUUVVVYZZ",
				lang: "fr",
				icon_add_player: icon_add_player,
				icon_labels: icon_labels,
				icon_remove_player: icon_remove_player,
				icon_scores: icon_scores,
				singleCard: true,
				singleCardRotation: 0,
				toolbarOpen: false,
				turn: 0,
				cardRandomRotate: true,
				cardStaticRotate: 0,
				showPlayerOptions: null
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

				// chooseType() will take care of choosing the letter too.
				this.chooseType();
				this.chooseColor();

				this.singleCardRotation = (Math.random() * 360);
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
				if ((this.playersNum + val) >= 2 && (this.playersNum + val) <= 6) {
					this.playersNum += val;
				}
			},

			changePlayerHandicap(player, val) {
				const num = player-1;
				this.handicapChangeDisabled = true;
				if (this.playerHandicaps[num] || this.playerHandicaps[num] === 0) {
					if (this.playerHandicaps[num] + val >= 0) {
						this.playerHandicaps[num] += val;
					}
				}
			},

			rotateCard90() {
				this.cardStaticRotate += 90;
				if (this.cardStaticRotate >= 360) {
					this.cardStaticRotate = 0;
				}
			},

			toggleScores() {
				this.showScores = !this.showScores;
			},

			toggleLabels() {
				this.showLabels = !this.showLabels;
			},

			togglePlayerOptions(i) {
				if (this.showPlayerOptions === i) {
					this.showPlayerOptions = null;
				} else {
					this.showPlayerOptions = i;
				}
			},

			toggleSingleCard() {
				this.singleCard = !this.singleCard;
			},

			toggleRandomRotate() {
				this.cardRandomRotate = !this.cardRandomRotate;
			},

			toggleOpenToolbar() {
				this.toolbarOpen = !this.toolbarOpen;
			}
		},

		watch: {
			playersNum() {
				switch (this.playersNum) {
					case 2:
						this.playerBoardsRotates = [0, 180];
						break;
					case 3:
						this.playerBoardsRotates = [0, 120, 240];
						break;
					case 4:
						this.playerBoardsRotates = [0, 90, 180, 270];
						break;
					case 5:
						this.playerBoardsRotates = [0, 72, 144, 216, 288];
						break;
					case 6:
						this.playerBoardsRotates = [0, 60, 120, 180, 240, 300];
						break;
					default:
				}

				for (let x=0; x<this.playersNum; x++) {
					//if (this.playerHandicaps[x]) {

					//}
				}

				if (this.playersNum >= 4) {
					this.singleCard = true;
					this.disableSingleCardBtn = true;
				} else {
					this.disableSingleCardBtn = false;
				}
			},
			handicapChangeDisabled() {
				/*
				Little sleight of hand to force the playerHandicaps array to detect an update ;
				apparently changing one value in an array in the data() doesn't count as reactive.
				*/
				this.handicapChangeDisabled = false;
			},
			turn() {
				if (this.turn > 0) {
					this.gameStarted = true;
				}
			},
		}
	}
</script>
