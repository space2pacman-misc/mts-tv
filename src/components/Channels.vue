<template>
	<div v-if="data" class="channels">
		<div class="channels__title">{{ data.channelDetails.length }} каналов</div>
		<div class="channels__list">
			<div 
				class="channel"
				v-for="(channel, index) in data.channelDetails.slice(0, range)"
				:key="index"
				@mouseover="showDescription"
			>
				<img class="channel__img" :src="channel.picture.backgrounds[0]" @error="onErrorBackground($event)">
				<div class="channel__description">
					<div class="channel__description-title">{{ channel.name }}</div>
					<img class="channel__description-icon" :src="channel.picture.channelPics[0]" @error="onErrorIcon($event)" />
					<div class="channel__description-text">{{ channel.introduce }}</div>
				</div>
			</div>
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
export default {
	data() {
		return {
			range: 24,
			amountLoad: 12
		}
	},
	methods: {
		onErrorBackground(event) {
			event.target.src = "https://on-desktop.com/images/wp.php?path=/wps/Creative_Wallpaper_Empty_channel_016564_.jpg&wp=";
		},
		onErrorIcon(event) {
			event.target.remove();
		},
		loadMore() {
			this.range += this.amountLoad;
		},
		showDescription() {
			
		}
	},
	props: {
		data: Object
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

.channel {
	background-size: 100%;
	border: 2px solid transparent;
	width: 160px;
	height: 96px;
	box-sizing: border-box;
	border-radius: 5px;
	margin-bottom: 23px;
	transition: all, .25s;
	cursor: pointer;
	position: relative;
}

.channel:hover {
	transform: translateY(-7px);
	border-color: #BA9D5A;
	z-index: 1;
}

.channel__description {
	display: none;
	width: 326px;
	background: #233345;
	position: absolute;
	left: 142px;
	top: 75px;
	padding: 30px 20px;
	box-sizing: border-box;
	z-index: 2;
}

.channel:hover .channel__description {
	display: block;
}

.channel__description-title {
	font-size: 20px;
	color: #fff;
}

.channel__description-icon {
	position: absolute;
	top: 10px;
	right: 10px;
	width: 40px;
	height: 40px;
}

.channel__description-text {
	color: #fff;
	font-size: 12px;
	line-height: 16px;
	margin-top: 15px;
}

.channel__img {
	width: 100%;
	height: 100%;
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