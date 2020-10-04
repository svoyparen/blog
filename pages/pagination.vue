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
      ref="pushMessage"
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
      page: 1,
    }),

    mounted() {
      this.getData()
    },

    methods: {
      async getData(page = this.page) {
        this.page = page
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

      showPushMessage(data) {
        this.$refs.pushMessage.setVisible(data['message'], data['type'])
        console.log( 'message: ' + data['message'] +  '\ntype: ' + data['type'])
        
      },

    },
  }
</script>
