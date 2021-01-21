<template>
	<div id="app" class="app">
		<div class="navbar">
			<Tabs :tabs="tabs.list" :active="tabs.active" @onSelected="onTabSelected" />
			<div class="dropdowns">
				<Dropdown title="Сортировка" :list="dropdownList.sort.list" :active="dropdownList.sort.value" @onSelected="onSortSelected" @onReset="onSortReset" />
				<Dropdown title="Телеканалы" :list="getGenres(data.channels)" :active="dropdownList.channels.value" @onSelected="onChannelSelected" @onReset="onChannelReset" />
			</div>
		</div>

		<First v-if="tabs.active === 'First'" />
		<Second v-if="tabs.active === 'Second'" />
		<Channels v-if="tabs.active === 'Телеканалы'" :data="data.channels" />
	</div>
</template>

<script>
import Tabs from "@/components/Tabs";
import Dropdown from "@/components/Dropdown";
import First from "@/components/First";
import Second from "@/components/Second";
import Channels from "@/components/Channels";

//test
import channelsData from "@/assets/channels.json";
//

export default {
	name: "App",
	data() {
		return {
			data: {
				channels: null
			},
			dropdownList: {
				sort: {
					list: ["По умолчанию", "По возрастанию", "По убыванию"],
					value: "По умолчанию"
				},
				channels: {
					list: ["Все телеканалы"],
					value: "Все телеканалы"
				}
			},
			tabs: {
				active: "Телеканалы",
				list: [
					{
						name: "First"
					},
					{
						name: "Second"
					},
					{
						name: "Телеканалы"
					}
				]
			}
		}
	},
	methods: {
		getGenres(channels) {
			if(channels) {
				let result = channels.channelDetails.map(channel => {
					if(channel.genres) {
						return channel.genres.reduce((list,genre) => {
							list.push(genre["genreName"]);
							
							return list;
						}, []);
					} else {
						return [];
					}
				}).flat();

				result.unshift("Все телеканалы");

				return Array.from(new Set(result));
			} else {
				return [];
			}
		},
		onSortSelected(value) {
			this.dropdownList.sort.value = value;

			if(value === "По умолчанию") {
				this.data.channels = channelsData;
			} else {
				this.data.channels.channelDetails.sort((a, b) => {
					if(value === "По возрастанию") {
						if(a.name > b.name) return 1;
						if(a.name < b.name) return -1;
					}

					if(value === "По убыванию") {
						if(a.name > b.name) return -1;
						if(a.name < b.name) return 1;
					}
				})
			}
		},
		onChannelSelected(value) {
			this.dropdownList.channels.value = value;
		},
		onSortReset() {

		},
		onChannelReset() {

		},
		onTabSelected(name) {
			this.tabs.active = name;
		}
	},
	mounted() {
		//test
		this.data.channels = channelsData;
		//
	},
	components: {
		Tabs,
		Dropdown,
		First,
		Second,
		Channels
	}
}
</script>

<style>
@font-face {
	font-family: "Roboto";
	font-weight: 400;
	src: url("~@/assets/fonts/Roboto-Regular.ttf");
}

@font-face {
	font-family: "Roboto";
	font-weight: 700;
	src: url("~@/assets/fonts/Roboto-Bold.ttf");
}

* {
	margin: 0;
	padding: 0;
}

li {
	list-style: none;
}

body {
	margin: 0;
	font-family: "Roboto";
}

.app {
	background: linear-gradient(180deg, rgba(92, 127, 227, 0) 12.76%, rgba(92, 127, 227, 0.2) 54.97%, rgba(211, 251, 244, 0) 75.49%), linear-gradient(90deg, #001424 0%, #001424 0.01%, rgba(30, 76, 104, 0.49) 50.22%, #001424 100%), rgba(35, 51, 69, 0.25);
	width: 1280px;
	margin: 0 auto;
	padding: 52px 100px 100px 140px;
	box-sizing: border-box;
	min-height: 100vh;
}

.navbar {
	display: flex;
	justify-content: space-between;
}

.dropdowns {
	display: flex;
}

.empty {
	font-size: 14px;
	color: rgba(232, 249, 255, 0.4);
	text-align: center;
	margin-top: 50px;
}
</style>