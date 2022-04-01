<template>
  <div class="game">
    <section class="gameboard">
      <ul class="gameboard__buttons">
        <li v-for="(item, idx) in layout" :key="idx" class="gameboard__button">
          <SimonButton
            :idx="idx"
            :color="item.color"
            :name="item.name"
            ref="button"
            @click="handleButtonClick"
          />
        </li>
      </ul>
    </section>
    <aside class="sidebar">
      <section class="play">
        <h1 class="play__round">Раунд: {{ round }}</h1>
        <button class="play__button" @click="init">Жми</button>
        <span v-show="gameover" class="play__result">Проиграли после {{ round }} раунда</span>
      </section>

      <section class="difficulty">
        <h2 class="difficulty__heading">Ур. Сложности</h2>
        <ul class="difficulty__list">
          <li v-for="(item, idx) in difficultyLevels" :key="idx" class="difficulty__item">
            <input
              v-model="difficulty"
              :value="item.value"
              :id="`difficulty-${item.value}`"
              type="radio"
              class="difficulty__input"
            />
            <label :for="`difficulty-${item.value}`">{{ item.text }}</label>
          </li>
        </ul>
      </section>
    </aside>
  </div>
</template>

<script>
	import SimonButton from '@/components/SimonButton';
	import { layout, difficultyLevels } from '@/shared/constants';
	import { sleep } from '@/shared/functions';

	export default {
		name: 'SimonG',
		components: {
			SimonButton,
		},
		data: () => ({
			layout,
			difficultyLevels,
			round: 0,
			current: 0,
			playSequence: [],
			prevButtonIdx: null,
			difficulty: 'easy',
			gameover: false,
		}),

		computed: {
			interval() {
				return this.difficultyLevels[this.difficulty].interval;
			},
		},

		methods: {
			init() {
				this.reset();
				this.generateNewRound();
			},

			reset() {
				this.round = 0;
				this.gameover = false;
				this.playSequence = [];
				this.prevButtonIdx = null;
			},

			async generateNewRound() {
				this.round += 1;
				this.current = 0;

				const idx = this.generateNextButtonIdx();
				this.prevButtonIdx = idx;
				this.playSequence.push(idx);

				for (const idx of this.playSequence) {
					this.$refs.button[idx].play();

					await sleep(this.interval);
				}
			},

			generateNextButtonIdx() {
				let num;
				do {
					num = Math.round(Math.random() * 3);
				} while (num === this.prevButtonIdx);

				return num;
			},

			handleButtonClick(idx) {
				if (!this.round) return;

				const currentIdx = this.playSequence[this.current];

				if (idx === currentIdx) {
					this.incrementCurrent();
				} else {
					this.gameover = true;
				}
			},

			async incrementCurrent() {
				if (this.current !== this.round - 1) {
					this.current += 1;
				} else {
					await sleep(1000);
					this.generateNewRound();
				}
			},
		},
	};
</script>

<style scoped>
	.game {
		display: flex;
		flex-direction: column;
		align-items: center;
	}

	@media screen and (min-width: 1224px) {
		.game {
			flex-direction: row;
		}
	}

	@media screen and (min-width: 1224px) {
		.gameboard {
			margin-right: 3rem;
		}
	}

	.gameboard__buttons {
		display: grid;
		grid-template-columns: 1fr 1fr;
		grid-template-rows: 1fr 1fr;
		padding: 0;
		margin: 0;
		height: 75vw;
		width: 75vw;
		max-height: 300px;
		max-width: 300px;
		list-style-type: none;
		box-shadow: 0 0 10px 2px rgba(0, 0, 0, 0.5);
	}

	@media screen and (min-width: 1224px) {
		.gameboard {
			margin-right: 3rem;
		}
	}

	.gameboard__button {
		display: block;
		height: 100%;
		width: 100%;
		overflow: hidden;
	}

	.sidebar {
		padding: 30px;
		min-width: 270px;
	}

	.play {
		margin-bottom: 2rem;
	}

	.play__round,
	.difficulty__heading {
		display: block;
		margin: 0;
		margin-bottom: 0.5rem;
		color: var(--heading-color);
		font-size: 2rem;
	}

	.difficulty__list {
		margin: 0;
		padding: 0;
		list-style-type: none;
	}

	.difficulty__item {
		font-size: 1rem;
		line-height: 1.5rem;
	}

	.play__button {
		display: block;
		padding: 0.5rem 1rem;
		min-width: 100px;
		font-size: 1rem;
	}

	.play__result {
		display: block;
		margin-top: 1rem;
	}

	.play,
	.difficulty {
		display: flex;
		flex-direction: column;
		align-items: center;
	}

	@media screen and (min-width: 1224px) {
		.play,
		.difficulty {
			display: block;
		}
	}
</style>
