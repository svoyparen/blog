<template>
	<div class="pushMessage" :class="type" v-if="this.isVisible === true">
			<div class="pushMessage__content">
				<p>{{ this.messageText }}</p>
			</div>
	</div>
</template>

<script>
	export default {

		data: () => ({
				isVisible: false,
				messageText: '',
				type: '',
			}),

		methods: {
			setVisible(message, type) {
				this.isVisible = true
				this.setData(message)
				this.type = type
				this.hidePushMessage()
			},

			setData(message) {
				this.messageText = message
			},

			hidePushMessage() {
				let context = this
				setTimeout( function() {
					context.isVisible = false
				}, 3000)
			}
		},

	}
</script>

<style scoped>
	.pushMessage {
		position: fixed;
		top: 100px;
		right: 26px;
		z-index: 10;
		padding: 15px 25px;
		border-radius: 5px;
		color: black;
		display: flex;
		justify-content: space-between;
		align-items: center;
		height: 50px;
		margin-bottom: 16px;
		border: 2px solid white;
		box-shadow: 0 0 10px rgba(0,0,0,0.5);
	}

	.ok {
		background-color: green;
	}

	.warning {
		background-color: orange;
	}

	.error {
		background-color: red;
	}

</style>