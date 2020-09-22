<template>
  <div class="container" ref="overflow">
    <Table2
      :items="items"
      
      @showPopup="openPopup"
      />
      <Popup
        ref="popup"
      />
<!--    <PushMessage
      @showPushMessage="showPush"
      ref="pushMessage"
    />
-->
  </div>
</template>

<script>
  import Table2 from '@/components/table2'
  import Popup from '@/components/popup'
  //import PushMessage from '~/components/PushMessage'
  import axios from 'axios'

  export default {
    components: {
      Table2,
      Popup,
      //PushMessage,
    },

    data: () => ({
      items: [],
      loading: false,
      nextRow: null,
      messageFlag: true,
      messageText: 'Статья добавлена',
    }),

    mounted() {
		this.getData()
    },

    methods: {

    	async getData() {
	      	while(this.nextRow !== -1){
				let response = await axios.get('https://test.cornapi.ru/blog', {params: { 'fromRow': this.nextRow }} )
	        	.catch( error => {
	        		console.log('Smthng wrong! ' + e)
	        	})
	        	response.data.data.items.forEach((item) => this.items.push(item))
	        	this.nextRow = response.data.data.nextRow
			}
		},
	
      openPopup(item) {
        this.$refs.popup.setVisible(true)
        this.$refs.popup.setData(item)
      },
/*
      showPush(message) {
        console.log('Show Push Message')
        this.$refs.pushMessage.setVisible(true)
        this.$refs.pushMessage.setData(message)
      },
*/
    },

  }

</script>

