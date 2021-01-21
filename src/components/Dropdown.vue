<template>
	<div class="dropdown">
		<div class="dropdown__button" :class="{ 'dropdown__button--active': isOpen }" @click="changeState">{{ title }}</div>
		<div class="dropdown__list" v-if="isOpen">
			<div 
				v-for="(item, index) in list"
				:key="index"
				class="dropdown__item"
				:class="{ 'dropdown__item--active': item === active }"
				@click="onSelect(item)"
			>
				{{ item }}
			</div>
			<div class="dropdown__item dropdown__reset" @click="onReset">Сбросить выбор</div>
		</div>
	</div>
</template>

<script>
export default {
	data() {
		return {
			isOpen: false,
		}
	},
	methods: {
		onSelect(value) {
			this.$emit("onSelected", value);
			this.isOpen = false;
		},
		onReset() {
			this.$emit("onReset");
			this.isOpen = false;
		},
		changeState() {
			this.isOpen = !this.isOpen;
		}
	},
	props: {
		title: String,
		list: Array,
		active: String
	}
}
</script>

<style scoped>
.dropdown {
	position: relative;
    margin-left: 22px;
}

.dropdown__button {
	border: 1px solid #415165;
	box-sizing: border-box;
	font-size: 17px;
	padding: 7px 12px;
	width: 165px;
	color: #BA9D5A;
	cursor: pointer;
	border-radius: 5px;
	color: #fff;
}

.dropdown__button--active {
	border-radius: 0px 0px 5px 5px;
	border-color: #BA9D5A;
	color: #BA9D5A;
}

.dropdown__list {
	position: absolute;
	overflow: hidden;
	background: #fff;
	margin-top: 6px;
	box-shadow: 0px 4px 4px rgba(0, 0, 0, 0.25);
	border-radius: 5px;
	width: 100%;
	z-index: 2;
	max-height: 200px;
	overflow: auto;
}

.dropdown__item {
	padding: 10px 0px 10px 18px;
	font-size: 15px;
	cursor: pointer;
}

.dropdown__item--active {
	background: #BA9D5A;
}

.dropdown__item:hover {
	background: #BA9D5A;
}

.dropdown__reset {
	text-align: center;
	background: #233345;
	font-size: 14px;
	color: #C1D2E9;
	padding: 10px 0px 10px 0px;
}
</style>