<template>
	<div class="spinner">
		<div class="spinner__box">
			<div class="circle circle-1">
				<div class="circle-inner"></div>
			</div>
			<div class="circle circle-2">
				<div class="circle-inner"></div>
			</div>
		</div>
	</div>
</template>

<style scoped>
.spinner {
	text-align: center;
    margin-top: 20px;
}

.spinner__box * {
	box-sizing: border-box;
}

.spinner__box {
	display: inline-block;
	animation-name: anim-spinner;
	animation-duration: .7s;
	animation-iteration-count: infinite;
	animation-timing-function: linear;
}

.circle {
	width: 3em;
	height: 1.5em;
	overflow: hidden;
}

.circle-inner {
	transform: rotate(45deg);
	border-radius: 50%;
	border: 0.375em solid white;
	border-right: 0.375em solid transparent;
	border-bottom: 0.375em solid transparent;
	width: 100%;
	height: 200%;
	animation-name: anim-circle-1;
	animation-duration: .7s;
	animation-iteration-count: infinite;
	animation-direction: alternate;
	animation-timing-function: cubic-bezier(.25, .1, .5, 1);
}

.circle-2 {
	transform: rotate(180deg);
}

.circle-2 .circle-inner {
	animation-name: anim-circle-2;
}

@keyframes anim-circle-1 {
	from {
		transform: rotate(60deg);
	}
	
	to {
		transform: rotate(205deg);
	}
}

@keyframes anim-circle-2 {
	from {
		transform: rotate(30deg);
	}
	
	to {
		transform: rotate(-115deg);
	}
}

@keyframes anim-spinner {
	from {
		transform: rotate(0deg);
	}
	
	to {
		transform: rotate(360deg);
	}
}
</style>