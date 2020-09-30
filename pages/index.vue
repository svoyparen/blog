<template>
  <div class="container" ref="overflow">
    <Table
      :items="items"
      
      @showPopup="openPopup"
      />
      <Popup
        ref="popup"
        @showMessage="showPushMessage"
      />
    <PushMessage
      ref="message"
    />
  </div>
</template>

<script>
  import Table from '@/components/table'
  import Popup from '@/components/popup'
  import PushMessage from '~/components/PushMessage'
  import axios from 'axios'

  export default {
    components: {
      Table,
      Popup,
      PushMessage,
    },

    data: () => ({
      items: [],
      loading: false,
      nextRow: null,
      messageFlag: true,
    }),

    mounted() {
      this.$refs.overflow.addEventListener('scroll', this.handleScroll, false)
      this.getData()
    },

    beforeDestroy() {
      this.$refs.overflow.removeEventListener('scroll', this.handleScroll, false)
    },

    methods: {
      async handleScroll(e) {
        const
          { scrollTop, scrollHeight, clientHeight } = e.target,
          scrollTopMax = scrollHeight - clientHeight

        if(scrollTopMax - scrollTop < 50) {
            await this.getData()
        }
      },

      async getData() {
        if(this.loading === false && this.nextRow != -1) {
          this.loading = true
          let response = await axios.get('https://test.cornapi.ru/blog', {params: { 'fromRow': this.nextRow }} )
          .catch( error => {
            console.log('Smthng wrong! ' + e)
          })
          response.data.data.items.forEach((item) => this.items.push(item))
          this.nextRow = response.data.data.nextRow
          this.loading = false
        }
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

