<template>
	<div v-loading="loading">
		<b-nav vertical class="w-25">
			<b-nav-item
				:active="activeTab(key)"
				v-for="key in Object.keys(categoryNames)"
				:key="key">
				{{ categoryNames[key] }}
			</b-nav-item>
		</b-nav>
		<b-container>
		</b-container>
	</div>
</template>

<script>
import axios from 'axios';

export default {
	name: 'Dashboard',

	data() {
		return {
			errors: [],
			categoryWiseData: {},
			categoryNames: {},
			loading: false,
			currentlyActive: 0,
		};
	},

	created() {
		this.loading = true;
		axios.get('https://thesmartq.firebaseio.com/menu.json')
			.then((response) => {
				const items = response.data[0];
				Object.keys(items).forEach((key) => {
					const data = items[key];
					if (this.categoryWiseData[data.category]) {
						this.categoryWiseData[data.category].push(data);
					}
					else {
						this.categoryNames[key] = data.category;
						this.categoryWiseData[data.category] = [];
						this.categoryWiseData[data.category].push(data);
					}
				});
				this.loading = false;
			})
			.catch((e) => {
				this.errors.push(e);
			});
	},

	activeTab(key) {
		if (this.currentlyActive === key) return true;
		return false;
	},
};
</script>
