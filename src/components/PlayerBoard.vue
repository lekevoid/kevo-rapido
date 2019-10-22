<template>
	<div class="player_board" :style="rotate">
		<Card :gameStarted="gameStarted" :currentCard="currentCard"></Card>
		<div class="scoreBoard">
			<button class="minus" @click="changeScore(-1)" :disabled="scoreChangeDisabled"><img :src="btn_minus" /></button>
			<div :class="['score', {hidden: showScores}]">{{ score }}</div>
			<button class="plus" @click="changeScore(1)" :disabled="scoreChangeDisabled"><img :src="btn_plus" /></button>
			<div :class="['confirm', confirmState, {hidden: !showScores}]">
				<img class="plus" :src="thumbsup" />
				<img class="minus" :src="sad" />
			</div>
		</div>
		<button class="rotate" v-dragged="rotateBoard"><img :src="rotate_arrows" /></button>
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
				confirmState: "",
				scoreChangeDisabled: false,
				btn_minus: btn_minus,
				btn_plus: btn_plus,
				rotate_arrows: rotate_arrows,
				thumbsup: thumbsup,
				sad: sad,
				boardIsRotating: false
			}
		},
		props: {
			gameStarted: Boolean,
			single: Boolean,
			showScores: Boolean,
			rotateValue: Number,
			currentCard: Object
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
						this.confirmState = "minus";
					} else {
						this.confirmState = "plus";
					}
					this.scoreChangeDisabled = true;

					this.score += val;
					setTimeout(() => { this.resetConfirmState() }, 1000);
				}
			},
			resetConfirmState() {
				this.confirmState = "";
				this.scoreChangeDisabled = false;
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

				console.log(this.rotateValue % 360)
			}
		},
		watch: {
			/*`
			rotateValue() {
				this.rotate = { transform: "rotate("+this.rotateValue+"deg)" }
			}
			*/
		}
	}
</script>