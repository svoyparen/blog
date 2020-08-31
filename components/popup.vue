<template>
	<div class="modal-mask" v-if="this.isVisible">
		<div class="modal-container">
			<form class="form">
				<label for="title">Название статьи</label><br />
				<input type="text" name="title" v-model.trim="title"><br />
				<label for="text">Текст статьи</label><br />
				<textarea name="text">{{ text }}</textarea><br />
				<span class="save" @click="save">Сохранить</span>
				<span class="reset" @click="reset">Сбросить</span>
			</form>
		</div>
	</div>
</template>

<script>
	export default {
		props: {
			id: { type: Number },
			title: { type: String, default: '' },
			text: { type: String, default: '' },
		},

		data: () => ({
			isVisible: false
		}),

		methods: {
			setVisible(flag) {
				console.log('Flag is ' + flag)
				this.isVisible = flag
			},

			setData(item) {
				this.title = item.title
				this.text = item.text
			},

			save() {
				console.log('save')
			},

			reset() {
				console.log('reset')
			}

		}
	}
</script>

<style scoped>
	.modal-mask {
		position: fixed;
		z-index: 9998;
		top: 0;
		left: 0;
		width: 100%;
		height: 100%;
		background-color: rgba(0, 0, 0, 0.7);
		display: table;
		transition: opacity 0.9s ease;
	}
	.modal-container {
		max-width: 600px;
		margin: 50px auto;
		background-color: #fff;
		border-radius: 10px;
		box-shadow: 0 2px 8px rgba(0, 0, 0, 0.33);
		transition: all 0.3s ease;
		font-family: Helvetica, Arial, sans-serif;
	}
	.form {
		width: 90%;
		padding: 15px;
		margin: 15px;
	}
	input {
		width: 100%;
		margin: 15px;
		padding: 15px;
	}
	textarea {
		width: 100%;
		margin: 15px;
		padding: 15px;

	}
	.save {
		margin: 15px;
		background-color: #00cc00;
		color: white;
		padding: 5px 15px;
	}
	.reset {
		margin: 15px;
		background-color: #cc3333;
		color: white;
		padding: 5px 15px;

	}
</style>