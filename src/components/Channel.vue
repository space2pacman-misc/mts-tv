<template>
	<div class="channel">
		<img v-show="isLoad" class="channel__img" :src="channel.picture.backgrounds[0]" @load="onLoadImage" @error="onErrorBackground($event)">
		<Spinner v-show="!isLoad" />
		<div class="channel__description">
			<div class="channel__description-title">{{ channel.name }}</div>
			<img class="channel__description-icon" :src="channel.picture.channelBlackWhites[0]" @error="onErrorIcon($event)" />
			<div class="channel__description-text">{{ channel.introduce }}</div>
		</div>
	</div>
</template>

<script>
import Spinner from "@/components/Spinner";

export default {
	data() {
		return {
			isLoad: false,
			src: ""
		}
	},
	watch: {
		channel() {
			this.isLoad = false;
		}
	},
	methods: {
		onErrorBackground(event) {
			event.target.src = "https://on-desktop.com/images/wp.php?path=/wps/Creative_Wallpaper_Empty_channel_016564_.jpg&wp=";
		},
		onErrorIcon(event) {
			event.target.remove();
		},
		onLoadImage() {
			this.isLoad = true;
		}
	},
	props: {
		channel: Object
	},
	components: {
		Spinner
	}
}
</script>

<style scoped>
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

@media (max-width: 1280px){
	.channel {
		width: 120px;
		height: 76px;
	}
}

@media (max-width: 768px) {
	.channel {
		width: 155px;
		height: 90px;
		margin-bottom: 12px;
	}
}
</style>