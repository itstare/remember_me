<template>
	<base-card>
		<base-button v-on:click="setSelectedTab('stored-resources')" v-bind:mode="storedResButtonMode">Stored Resources</base-button>
		<base-button v-on:click="setSelectedTab('add-resource')" v-bind:mode="addResButtonMode">Add Resource</base-button>
	</base-card>
	<keep-alive>
		<component v-bind:is="selectedTab"></component>
	</keep-alive>
</template>

<script>
	import StoredResources from './StoredResources.vue';
	import AddResource from './AddResource.vue';

	export default {
		components: {
			'stored-resources': StoredResources,
			'add-resource': AddResource
		},

		data() {
			return {
				selectedTab: 'stored-resources',
				storedResources: [
					{
						id: 'official-guide',
						title: 'Official Guide',
						description: 'The official Vue.js documentation.',
						link: 'https://vuejs.org'
					},
					{
						id: 'google',
						title: 'Google',
						description: 'Learn to google...',
						link: 'https://google.com'
					}
				]
			}
		},

		methods: {
			setSelectedTab(tab) {
				this.selectedTab = tab;
			},
			addResource(title, description, url) {
				const newResource = {
					id: new Date().toISOString(),
					title: title,
					description: description,
					link: url
				};
				this.storedResources.unshift(newResource);
				this.selectedTab = 'stored-resources';
			},
			deleteResource(resourceId) {
				const resIndex = this.storedResources.findIndex(res => res.id === resourceId);
				this.storedResources.splice(resIndex, 1);
			}
		},

		computed: {
			storedResButtonMode() {
				if(this.selectedTab === 'stored-resources') {
					return null;
				} else {
					return 'flat';
				}
			},

			addResButtonMode() {
				if(this.selectedTab === 'add-resource') {
					return null;
				} else {
					return 'flat';
				}
			}
		},

		provide() {
			return {
				resources: this.storedResources,
				addResource: this.addResource,
				deleteResource: this.deleteResource
			}
		}
	}
</script>