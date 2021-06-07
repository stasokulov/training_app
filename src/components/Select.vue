<template>
  <div class="select-wrap">
	<div>{{title}}</div>
	<select class="select" v-model=selectedNum @input="sendMum" >
		<option v-for="(option, index) in options" :key="index">
			{{option}}
		</option>
	</select>
  </div>
</template>

<script>
export default {
	name: 'Select',
	props: {
		title: String,
		options: Array
	},
	data() {
		return {
			selectedNum: null
		}
	},
	methods: {
		sendMum: function() {
			// setTimeout нужен, иначе emit отправит событие и значение раньше, чем значение заменится на актуальное.
			setTimeout(() => {
				this.$emit('input-num', this.selectedNum)
			},0);
		}
	}
}
</script>

<style scoped>
.select-wrap {
	width: 130px;
	padding: 10px;
	font-size: 16px;
}
.select {
	padding: 25px;
	height: 100px;
	font-size: 40px;
}
</style>
