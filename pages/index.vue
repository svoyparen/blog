<template>
  <div class="container" ref="overflow">
    <Table
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
  import Table from '@/components/table'
  import Popup from '@/components/popup'
  //import PushMessage from '~/components/PushMessage'
  import axios from 'axios'

  export default {
    components: {
      Table,
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
      this.$refs.overflow.addEventListener('scroll', this.handleScroll, false)
      this.getData()
    },

    beforeDestroy() {
      this.$refs.overflow.removeEventListener('scroll', this.handleScroll, false)
    },

    methods: {
      handleScroll(e) {
        const
          { scrollTop, scrollHeight, clientHeight } = e.target,
          scrollTopMax = scrollHeight - clientHeight

        if(scrollTopMax - scrollTop <= 50) {
          if(this.loading === false && this.nextRow != -1) {
            this.loading = true
            this.getData()
            this.loading = false
          }
        }
      },

      getData() {
        axios.get('https://test.cornapi.ru/blog', {params: { 'fromRow': this.nextRow }} )
        .then( response => {
          for ( const item in response.data.data.items ) {
            if ( this.items.includes(response.data.data.items[item]) == false )
              this.items.push(response.data.data.items[item])
          }
          this.nextRow = response.data.data.nextRow
        })
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

