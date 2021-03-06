<template>
	<div id="app" class="background">
		<div class="app">
			<div class="navbar">
				<Tabs :tabs="tabs.list" :active="tabs.active" @onSelected="onTabSelected" />
				<div class="dropdowns">
					<Dropdown title="Сортировка" :list="dropdownList.sort.list" :active="dropdownList.sort.value" @onSelected="onSortSelected" @onReset="onSortReset" />
					<Dropdown title="Телеканалы" :list="dropdownList.channels.list" :active="dropdownList.channels.value" @onSelected="onChannelSelected" @onReset="onChannelReset" />
				</div>
			</div>

			<First v-if="tabs.active === 'First'" />
			<Second v-if="tabs.active === 'Second'" />
			<Channels v-if="tabs.active === 'Телеканалы'" :data="data.channels.list" />
		</div>
	</div>
</template>

<script>
import Tabs from "@/components/Tabs";
import Dropdown from "@/components/Dropdown";
import First from "@/components/First";
import Second from "@/components/Second";
import Channels from "@/components/Channels";

export default {
	name: "App",
	data() {
		return {
			data: {
				channels: {
					list: null,
					raw: null
				}
			},
			dropdownList: {
				sort: {
					list: ["По умолчанию", "По возрастанию", "По убыванию"],
					value: "По умолчанию"
				},
				channels: {
					list: [],
					default: ["Все телеканалы"],
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

				return Array.from(new Set(result));
			} else {
				return [];
			}
		},
		saveLocalStorage(key, value) {
			localStorage[key] = value;
		},
		loadLocalStorage(key) {
			return localStorage[key];
		},
		clearLocalStorage(key) {
			delete localStorage[key];
		},
		request(url) {
			return fetch(`${window.location.href}/${url}`).then(response => {
				return response.json();
			});
		},
		onSortSelected(value) {
			this.dropdownList.sort.value = value;

			if(value === "По умолчанию") {
				this.data.channels.list = JSON.parse(JSON.stringify(this.data.channels.raw));
				this.clearLocalStorage("sort");
			} else {
				this.data.channels.list.channelDetails.sort((a, b) => {
					if(value === "По возрастанию") {
						if(a.name > b.name) return 1;
						if(a.name < b.name) return -1;
					}

					if(value === "По убыванию") {
						if(a.name > b.name) return -1;
						if(a.name < b.name) return 1;
					}
				})

				this.saveLocalStorage("sort", value);
			}
		},
		onChannelSelected(value) {
			this.dropdownList.channels.value = value;

			if(value === "Все телеканалы") {
				this.data.channels.list.channelDetails = this.data.channels.raw.channelDetails.slice();
				this.clearLocalStorage("channels");
			} else {
				this.data.channels.list.channelDetails = this.data.channels.raw.channelDetails.filter(channel => {
					if(channel.genres) {
						return channel.genres.find(genre => genre.genreName === value)
					} else {
						return false;
					}
				})

				this.saveLocalStorage("channels", value);
			}
		},
		onSortReset() {
			this.dropdownList.sort.value = "По умолчанию";
			this.data.channels.list = JSON.parse(JSON.stringify(this.data.channels.raw));
			this.clearLocalStorage("sort");
		},
		onChannelReset() {
			this.dropdownList.channels.value = "Все телеканалы";
			this.data.channels.list.channelDetails = this.data.channels.raw.channelDetails.slice();
			this.clearLocalStorage("channels");
		},
		onTabSelected(name) {
			this.tabs.active = name;
		}
	},
	mounted() {
		this.request("channels.json").then(response => {
			this.data.channels.raw = JSON.parse(JSON.stringify(response));
			this.data.channels.list = JSON.parse(JSON.stringify(response));
			this.dropdownList.channels.list = this.getGenres(this.data.channels.list);
			this.dropdownList.channels.list.unshift(...this.dropdownList.channels.default);

			let sort = this.loadLocalStorage("sort");
			let channels = this.loadLocalStorage("channels");

			if(channels) {
				this.onChannelSelected(channels);
			}

			if(sort) {
				this.onSortSelected(sort);
			}

		})
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

.background {
	min-width: 320px;
	background: linear-gradient(180deg, rgba(92, 127, 227, 0) 12.76%, rgba(92, 127, 227, 0.2) 54.97%, rgba(211, 251, 244, 0) 75.49%), linear-gradient(90deg, #001424 0%, #001424 0.01%, rgba(30, 76, 104, 0.49) 50.22%, #001424 100%), rgba(35, 51, 69, 0.25);
}

.app {
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

@media (max-width: 1280px){
	.app {
		width: 768px;
	}

	.navbar {
		display: block;
	}
}

@media (max-width: 768px){
	#app {
		overflow: hidden;
	}

	.app {
		width: 320px;
		padding: 52px 15px 100px 15px;
	}
}
</style>