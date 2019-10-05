<template>
	<div class="player_board">
		<button class="minus" @click="changeScore(-1)" :disabled="scoreChangeDisabled"><img :src="btn_minus" /></button>
		<div :class="['score', {hidden: showScores}]">{{ score }}</div>
		<button class="plus" @click="changeScore(1)" :disabled="scoreChangeDisabled"><img :src="btn_plus" /></button>
		<div :class="['confirm', confirmState, {hidden: !showScores}]">
			<img class="plus" :src="thumbsup" />
			<img class="minus" :src="sad" />
		</div>
	</div>
</template>

<script>
	import btn_minus from "../img/btn_minus.svg";
	import btn_plus from "../img/btn_plus.svg";
	import thumbsup from "../img/icon_thumbsup.svg";
	import sad from "../img/icon_sad.svg";

	export default {
		name: "Card",
		data() {
			return {
				score: 0,
				confirmState: "",
				scoreChangeDisabled: false,
				btn_minus: btn_minus,
				btn_plus: btn_plus,
				thumbsup: thumbsup,
				sad: sad,
			}
		},
		props: {
			showScores: Boolean
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
			}
		}
	}
</script>