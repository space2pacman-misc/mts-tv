<template>
	<div v-if="data" class="channels">
		<div class="channels__title">{{ data.channelDetails.length }} каналов</div>
		<div class="channels__list">
			<Channel v-for="(channel, index) in data.channelDetails.slice(0, range)" :key="index" :channel="channel" />
		</div>
		<div class="channels__bottom">
			<div class="channels__button-more" @click="loadMore">
				<div class="channels__button-more-icon"></div>
				<div class="channels__button-more-text">Показать еще</div>
			</div>
		</div>
	</div>
</template>

<script>
import Channel from "@/components/Channel";

export default {
	data() {
		return {
			range: 24,
			amountLoad: 12
		}
	},
	methods: {
		loadMore() {
			this.range += this.amountLoad;
		}
	},
	props: {
		data: Object
	},
	components: {
		Channel
	}
}
</script>

<style scoped>
.channels {
	margin-top: 20px;
}

.channels__title {
	font-size: 18px;
	color: #fff;
}

.channels__list {
	display: flex;
	flex-wrap: wrap;
	justify-content: space-between;
	margin-top: 23px;
}

.channels__bottom {
	text-align: center;
}

.channels__button-more {
	display: inline-block;
	text-align: center;
	cursor: pointer;
}

.channels__button-more-icon {
	background: url("~@/assets/icons/reload.svg") no-repeat;
	width: 28px;
	height: 28px;
	display: inline-block;
}

.channels__button-more-text {
	text-align: center;
	font-size: 14px;
	margin-top: 8px;
	color: rgba(232, 249, 255, 0.4);
}
</style>