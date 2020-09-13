<template>
	<div class="pushMessage">
		<transition name="fade" class="message">
			<p v-if="this.isVisible">{{ this.messageText }}</p>
		</transition>
	</div>
</template>

<script>
	export default {

		data: () => ({
					isVisible: false,
					messageText: '',
				}),

		methods: {
			setVisible(flag) {
				this.isVisible = flag
			},

			setData(message) {
				this.messageText = message
			},
		},

	}
</script>

<style scoped>
	.pushMessage {
		position: fixed;
		top: 80px;
		right: 16px;
		z-index: 10;
		padding: 15px 25px;
		background-color: green;
		border-radius: 5px;
		color: #ffffff;
		display: flex;
		justify-content: space-between;
		align-items: center;
		border: 2px solid white;
		box-shadow: 0 0 10px rgba(0,0,0,0.5);
		}
			.fade-enter-active, .fade-leave-active {
				transition: opacity .5s;
			}
			.fade-enter, .fade-leave-to  {
				opacity: 0;
			}
	.message {
		display: flex;
		flex-direction: column;
	}
	.message-enter {
		transform: translateX(120px);
		opacity: 0;
	}
	.message-enter-active {
		transition: all .6s ease;
	}
	.message-enter-to {
		opacity: 1;
	}
	.message-leave {
		opacity: 1;

	}
	.message-leave-active {
		transition: all .6s ease, opacity .6s, height .6s .2s;
	}
	.message-leave-to {
		height: 0;
		transform: translateX(120px);
		opacity: 0;
	}
	.message-move {
		transition: all .6s ease;
	}
</style>