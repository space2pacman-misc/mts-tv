<template>
	<ul class="tabs">
		<li 
			v-for="(tab, index) in tabs"
			class="tab"
			:key="index"
			:class="{ 'tab--active': tab.name === active }"
			@click="selectTab(tab.name)"
		>
			{{ tab.name }}
		</li>
	</ul>
</template>

<script>
export default {
	methods: {
		selectTab(name) {
			this.$emit("onSelected", name);
		}
	},
	props: {
		tabs: Array,
		active: String
	}
}
</script>

<style scoped>
.tabs {
	display: flex;
	padding: 0px 0px;
	margin-top: 8px;
	width: 481px;
}

.tab {
	font-size: 14px;
	border-bottom: 2px solid transparent;
	padding-bottom: 15px;
	color: #fff;
	width: 100%;
	text-align: center;
	cursor: pointer;
}

.tab:hover {
	color: #BA9D5A;
}

.tab--active {
	font-weight: 700;
	border-color: #BA9D5A;
}

.tab--active:hover {
	color: #fff;
}
</style>