<template>
	<tr class="tableRow" @click="showPopupToEditPost(item)">
		<td>{{ item.id }}</td>
		<td>{{ item.title }}</td>
		<td>{{ item.text }}</td>
		<td>{{ item.createdAt | toLocaleDateString }}</td>
		<td>{{ item.updatedAt | toLocaleDateString }}</td>
	</tr>
</template>

<script>
	export default {
		props: {
			item: { type: Object, default: () => { return [] } }
		},

		filters: {
			toLocaleDateString(date) {
				if( date ) {
					return new Date(date * 1000).toDateString()
				}
			},
		},

		methods: {
			showPopupToEditPost(item) {
				this.$emit('showPopup', item)
			},
		},

	}
</script>
