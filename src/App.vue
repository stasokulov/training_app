<template>
  <div id="app">
		<div class="start-area" v-if=state.start>
			<div class="selects">
				<Select :title=ranks.title
					:options=ranks.options
					@input-num=setRanks
				/>
				<Select :title=time.title
					:options=time.options
					@input-num=setTime
				/>
			</div>
			<button class="button" @click="showNum">Старт</button>
		</div>
		<div class="num-area" v-if=state.showNum>
			{{num}}
		</div>
		<div class="user-answer-area" v-if=state.userAnswer>
			<p>Введите число</p>
			<input class="answer-input" type="number" v-model=userAnswer>
			<button class="button" @click="checkNum">Проверить</button>
		</div>
		<div v-if="state.final">
			<p v-if="answer.exists && answer.ok">Верно!</p>
			<p v-if="answer.exists && !answer.ok">Не правильно. Попробуй еще.</p>
			<button class="button" @click="restart">Заново</button>
		</div>	
  </div>
</template>

<script>
import Select from './components/Select.vue'

export default {
	name: 'App',
	components: {
		Select
	},
	data() {
		return {
			ranks: {
				title: 'Разряды',
				options: [2,3,4,5],
				value: null
			},
			time: {
				title: 'Секунд показа',
				options: [0.2,0.5,1,1.5,2],
				value: null
			},
			state: {
				start: true,
				showNum: false,
				userAnswer: false,
				final: false
			},
			num: null,
			userAnswer: null,
			answer: {
				exists: false,
				ok: null
			}
		}
	},
	methods: {
		setRanks: function(num) {
			this.ranks.value = num;
		},
		setTime: function(num) {
			this.time.value = num;
		},
		hideAll: function() {
			for(let key in this.state) {
				this.state[key] = false;
			}
		},
		showNum: function() {
			this.hideAll();
			this.state.showNum = true;
			const random = Math.random();
			const ranks = Math.pow(10, this.ranks.value-1);
			const min = 1;
			const max = 10;
			const base = min + random * (max + 1 - min);
			this.num = Math.floor(base * (ranks));
			setTimeout(this.showUserAnswer, this.time.value*1000);
		},
		showUserAnswer: function() {
			this.hideAll();
			this.state.userAnswer = true;
		},
		checkNum: function() {
			this.hideAll();
			this.state.final = true;
			this.answer.ok = +this.userAnswer === this.num;
			this.answer.exists = true;
		},
		restart: function() {
			this.hideAll();
			this.state.start = true;
			this.ranks.value = null;
			this.time.value = null;
			this.num = null;
			this.userAnswer = null;
			this.answer.exists = false;
			this.answer.ok = null;
		}
	}
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  font-size: 40px;
}
.selects {
	display: flex;
	justify-content: center;
	margin-bottom: 20px;
}
.button {
	padding: 10px;
	font-size: 20px;
}
.user-answer-area {
	display: flex;
	flex-direction: column;
	align-items: center;
}
.answer-input {
	margin-bottom: 10px;
	padding: 25px;
	width: 140px;
    height: 100px;
    font-size: 40px;
}
</style>
