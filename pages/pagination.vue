<template>
  <div class="container" ref="overflow">

    <Table2
      :items="items"
      @showPopup="openPopup"
      @getData="getData"
      :quantityPages="quantityPages"
    />
    <Popup
      ref="popup"
      @getData="getData"
      @showMessage="showPushMessage"
    />
    <PushMessage
      ref="message"
    />

  </div>
</template>

<script>
  import Table2 from '@/components/table2'
  import Popup from '@/components/popup'
  import PushMessage from '~/components/PushMessage'
  import axios from 'axios'

  export default {
    components: {
      Table2,
      Popup,
      PushMessage,
    },

    data: () => ({
      items: [],
      loading: false,
      nextRow: null,
      quantityPages: 1,
      messageFlag: true,
    }),

    mounted() {
		  this.getData()
    },

    methods: {

    	async getData(page = 1) {
				let response = await axios.get('https://test.cornapi.ru/blog/paginate', {params: { 'page': page }} )
	        	.catch( error => {
	        		console.log('Smthng wrong! ' + e)
	        	})
        this.items = []
      	response.data.data.items.forEach((item) => this.items.push(item))
      	this.nextRow = response.data.data.nextRow
        this.quantityPages = response.data.data.quantityPages

		},
	
      openPopup(item) {
        this.$refs.popup.setVisible(true)
        this.$refs.popup.setData(item)
      },

      showPushMessage(message) {
        this.$refs.message.setVisible(message)
      },
    },

  }

</script>

