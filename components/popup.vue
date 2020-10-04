<template>
	<div class="modal-mask" v-if="this.isVisible">
		<div class="modal-container">
			<fieldset :disabled=isPostSend>
				<form class="form">
					<div class="close" @click="close">x</div>
					<div class="forms_block">
						<label for="title" class="label_title">Название статьи</label><br />
						<input type="text" name="title" v-model.trim="title"><br />
						<label for="text">Текст статьи</label><br />
						<textarea name="text" v-model.trim="text">{{ text }}</textarea><br />
						<div class="save" @click="save">Сохранить</div>
						<div class="remove" @click="remove">Удалить</div>
					</div>
				</form>
			</fieldset>
		</div>
	</div>
</template>

<script>
	import axios from 'axios'
	export default {
		data: () => ({
			isVisible: false,
			isPostSend: false,
			isPostDelete: false,
			id: '',
			title: '',
			text: '',
			messages: {
				saved: 'запись сохранена',
				updated: 'запись обновлена',
				removed: 'запись удалена',
			},
		}),
		methods: {
			setVisible(flag) {
				this.isVisible = flag
			},

			setData(item) {
				this.id = item.id
				this.title = item.title
				this.text = item.text
			},

			async save(event) {
				this.isPostSend = true
				let { id, title, text } = this
				if( !id ) {
					let response = await axios.post('https://test.cornapi.ru/blog', {title, text})
						.catch( error => {
							return false
						})
					if (response.status == '200') {
						// sendMessage(1); message ans type in Array variable
						this.$emit('showMessage', { message: this.messages['updated'], type: 'warning' })
					} else {
						return false
					}
				} else {
					let response = await axios.put('https://test.cornapi.ru/blog/' + id, {title, text})
						.catch( error => {
							return false
						})
					if (response.status == '200') {
						this.$emit('showMessage', { message: this.messages['updated'], type: 'warning' })
					} else {
						return false
					}
				}
				this.clearData()
				this.isPostSend = false
				this.setVisible(false)
				this.$emit('getData')
			},

			async remove(event) {
				this.isPostDelete = false
				let response = await axios.delete('https://test.cornapi.ru/blog/' + this.id)
					.then(response => {
						this.setVisible(false)
					})
					.catch( error => {
						return false
					})
				this.isPostDelete = true
				this.$emit('showMessage', { message : this.messages['removed'], type: 'error'})
				this.setVisible(false)
				this.$emit('getData')
			},

			close() {
				this.setVisible(false)
			},

			clearData() {
				this.id = ''
				this.title = ''
				this.text = ''
			}

		}
	}
</script>

<style scoped>
	.modal-mask {
		position: fixed;
		z-index: 998;
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
		overflow-y:auto;
	}
	.form {
		width: 90%;
		padding: 15px;
		margin: 15px;
		position: relative;
	}
	.forms_block {
		margin-top: 35px; 
	}
	label {
		color: #111111;
	}
	input {
		width: 100%;
		margin: 15px;
		padding: 15px;
	}
	textarea {
		width: 100%;
		height: 250px;
		margin: 15px;
		padding: 15px;
	}
	.save {
		margin: 15px;
		background-color: #00cc00;
		color: white;
		padding: 5px 15px;
		width: 50%;
	}
	.save:hover {
		background-color: #00aa00;
	}
	.remove {
		margin: 15px;
		background-color: #cc3333;
		color: white;
		padding: 5px 15px;
		width: 50%;
	}
	.remove:hover {
		background-color: #aa1111;
	}
	.close {
		width: 25px;
		height: 25px;
		text-align: center;
		border-radius: 5px;
		font-size: 1.3em;
		background-color: #000;
		color: white;
		position: absolute;
		top: 15px;
		right: 0;
		cursor: pointer;
		font-weight: bold;
	}
</style>