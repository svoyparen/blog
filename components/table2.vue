<template>
	<div id="table">
		<table class="table">
			<thead class="thead">
				<th>Номер статьи</th>
				<th>Заголовок статьи</th>
				<th>Текст статьи</th>
				<th>Дата публикации</th>
				<th>Дата обновления</th>
			</thead>
			<tfoot class="tfoot">
				<td>Всего статей</td>
				<td>{{ items.length }}</td>
			</tfoot>
			<tbody>
				<TableRow
					v-for="item in items"
					:key="item.id"
					:item="item"
					@showPopup="showPopupToEditPost"
				/>
			</tbody>
		</table>
		
		<span
			class="pages"
			v-for="page in quantityPages"
			:key="page"
			:class="{ 'pageSelected': page === pageNumber }"
			@click="goTo(page)">
				{{ page }}
		</span>
		
	</div>
</template>

<script>
	import TableRow from '~/components/TableRow'
	export default {
		props: {
			items: { Array, required: true },
			quantityPages: { Number, required: true },
		},

		components: {
			TableRow,
		},

		data: () => ({
			pageNumber: 1,
		}),

		methods: {

			showPopupToEditPost(item) {
				this.$emit('showPopup', item)
			},

			goTo(page) {
				this.pageNumber = page
				this.$emit('getData', page)
			},

		}
	}
</script>

<style>
	#table {
		width: 100%;
		background-color: #fff;
		display: block;

	}
	thead {
		background-color: #fc0;
		border: none;
		/*position: sticky;*/
		top: 0;
	}
	tr {
		background-color: #ffc;
		border: none;
	}
	th, td {
		padding: 10px;
		border-bottom: 1px solid black;
	}
	tr:hover {
		background-color: #ccffcc;
	}
	.table {
		position: relative;
	}
	.tfoot {
		background-color: #f0c0a0;
		
	}
	.thead {
	}
	.pages {
		border: 1px solid grey;
		font-size: 1.1em;
		margin: 5px;
		padding: 5px;
		text-align: center;
	}
	.pages:hover, .pageSelected {
		background-color: #fc0;
		cursor: pointer;
		color:white;
	}
</style>