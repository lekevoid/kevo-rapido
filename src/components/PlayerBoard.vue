<template>
	<div class="player_board" :style="rotate">
		<Card :gameStarted="gameStarted" :currentCard="currentCard"></Card>
		<div class="options">
			<div class="row">
				<button class="minus" @click="changeScore(-1)" :disabled="scoreChangeDisabled"><img :src="btn_minus" /></button>
				<div :class="['score', {hidden: showScores}]">{{ score }}</div>
				<button class="plus" @click="changeScore(1)" :disabled="scoreChangeDisabled"><img :src="btn_plus" /></button>
				<div :class="['confirm_score_change', confirmScoreChangeState, {hidden: !showScores}]">
					<img class="plus" :src="thumbsup" />
					<img class="minus" :src="sad" />
				</div>
			</div>
			<div class="row">
				<button class="rotate" v-dragged.prevent="rotateBoard"><img :src="rotate_arrows" /></button>
				<button class="toggle_options" @click="toggleOptions()">Options</button>
			</div>
		</div>
	</div>
</template>

<script>
	import Card from "./Card.vue";

	import btn_minus from "../img/btn_minus.svg";
	import btn_plus from "../img/btn_plus.svg";
	import thumbsup from "../img/icon_thumbsup.svg";
	import rotate_arrows from "../img/rotate_arrows.png";
	import sad from "../img/icon_sad.svg";

	export default {
		name: "PlayerBoard",
		components: {
			Card
		},
		data() {
			return {
				score: 0,
				confirmScoreChangeState: "",
				scoreChangeDisabled: false,
				btn_minus: btn_minus,
				btn_plus: btn_plus,
				rotate_arrows: rotate_arrows,
				thumbsup: thumbsup,
				sad: sad,
				boardIsRotating: false,
				rotateValue: this.rotateOrigValue,
			}
		},
		props: {
			playerNum: Number,
			gameStarted: Boolean,
			single: Boolean,
			showScores: Boolean,
			rotateOrigValue: Number,
			currentCard: Object,
			turn: Number
		},
		computed: {
			rotate() {
				return {
					transform: "translateX(-50%) rotate("+this.rotateValue+"deg)"
				};
			}
		},
		methods: {
			changeScore(val) {
				if (this.score + val >= 0) {
					if (val < 0) {
						this.confirmScoreChangeState = "minus";
					} else {
						this.confirmScoreChangeState = "plus";
					}
					this.scoreChangeDisabled = true;

					this.score += val;
					setTimeout(() => { this.resetConfirmState() }, 500);
				}
			},
			resetConfirmState() {
				this.confirmScoreChangeState = "";
				this.scoreChangeDisabled = false;
				this.handicapChangeDisabled = false;
			},
			rotateBoard({ el, deltaX, deltaY, offsetX, offsetY, clientX, clientY, first, last }) {
				if (first) {
					this.isDragging = true;
					return;
				}
				if (last) {
					this.isDragging = false;
					return;
				}

				if (this.rotateValue % 360 < 90) {
					this.rotateValue = this.rotateValue - (deltaX / 5) - (deltaY / 5);
				} else if (this.rotateValue % 360 < 180) {
					this.rotateValue = this.rotateValue + (deltaX / 5) - (deltaY / 5);
				} else if (this.rotateValue % 360 < 270) {
					this.rotateValue = this.rotateValue + (deltaX / 5) + (deltaY / 5);
				} else {
					this.rotateValue = this.rotateValue - (deltaX / 5) + (deltaY / 5);
				}

				if (this.rotateValue > 360) {
					this.rotateValue = 0;
				}

				if (this.rotateValue < 0) {
					this.rotateValue = 359;
				}
			},
			toggleOptions() {
				this.$parent.togglePlayerOptions(this.playerNum);
			}
		},
		watch: {
			/*
			rotateValue() {
				console.log(this.rotateValue);
			}
			*/
		}
	}
</script>