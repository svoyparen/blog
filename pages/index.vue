<template>
  <div class="container">
    <PushMessage
      @showPushMessage="showPush"
      ref="pushMessage"
    />
    <Table
      :items="items"
      @showPopup="openPopup"
      />
      <Popup
        ref="popup"
      />
  </div>
</template>

<script>
  import Table from '~/components/table'
  import Popup from '~/components/popup'
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
      fromRow: null,
      messageFlag: true,
      messageText: 'Статья добавлена',
    }),

    mounted() {
      this.getData()
    },

    methods: {
      getData() {
        let from = this.fromRow
        axios.get('https://test.cornapi.ru/blog', {params: { from }} )
        .then( response => {
          for ( const item in response.data.data.items ){
            this.items.push(response.data.data.items[item])
          }
        })
        //this.fromRow = this.fromRow + 10
      },

      openPopup(item) {
        this.$refs.popup.setVisible(true)
        this.$refs.popup.setData(item)
      },

      showPush(message) {
        console.log('Show Push Message')
        this.$refs.pushMessage.setVisible(true)
        this.$refs.pushMessage.setData(message)
      }

    },

    }

</script>

<style>
.container {
  margin: 0 auto;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
  min-width: 1000px;
  background-color: #159159;
}

.title {
  font-family:
    'Quicksand',
    'Source Sans Pro',
    -apple-system,
    BlinkMacSystemFont,
    'Segoe UI',
    Roboto,
    'Helvetica Neue',
    Arial,
    sans-serif;
  display: block;
  font-weight: 300;
  font-size: 100px;
  color: #35495e;
  letter-spacing: 1px;
}

.subtitle {
  font-weight: 300;
  font-size: 42px;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
}

.links {
  padding-top: 15px;
}
</style>
