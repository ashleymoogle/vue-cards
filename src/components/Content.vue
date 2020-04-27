<template>
  <div class="wrapper">
    <div class="title">
      Jeu de carte
    </div>
    <div class="desc">
      Some description that is supposed to be somewhat long or something
    </div>
    <div class="data">
      <div> selected: {{ selected.length }}</div>
      <div>
        Total value: {{ total }}
      </div>
    </div>
    <div v-if="!isLoading" class="cards">
      <div :class="`card ${cardColor(card.suit)}`" v-for="(card, index) in cards" :key="index">
        <div class="card-name">
          {{card.name}} of {{ card.suit }}
        </div>
        <img :src="card.image" :alt="card.name" />
        <div class="button fill" v-if="!isSelected(cardId(card.name, card.suit))" @click="select(cardId(card.name, card.suit))">
          Select
        </div>
        <div class="button fill" v-else @click="unSelect(cardId(card.name, card.suit))">
          Unselect
        </div>
      </div>
    </div>
    <div v-else>
      Loading...
    </div>
  </div>
</template>

<script>
  import mapping from '../mapping.js'
  export default {
    name: "Content",
    props: {
      order: {
        default: 'default',
        type: String
      }
    },
    data () {
      return {
        isLoading: true,
        original: [],
        cards: [],
        selected: []
      }
    },
    computed: {
      total () {
        return this.selected.reduce((acc, item) => {
          acc += parseInt(mapping[item.split('-')[0]])
          return acc
        }, 0)
      }
    },
    watch: {
      order: function (nv) {
        const newVal = nv.split('-')[0]
        switch (newVal) {
          case 'suits':
            this.suits()
            break;
          case 'value':
            this.value()
            break;
          case 'shuffle':
            this.shuffle()
            break;
          default:
            this.cards = this.original
        }
      }
    },
    mounted () {
      fetch('https://wewebdev.s3-eu-west-1.amazonaws.com/public/senior-front-test/cards.json')
        .then((response) => {
          return response.json();
        })
        .then((data) => {
          this.isLoading = false
          this.original = data
          this.cards = data.reduce((acc, item) => {
            item.value = parseInt(mapping[item.name])
            acc.push(item)
            return acc
          }, [])
        });
    },
    methods: {
      isSelected (cardId) {
        return this.selected.find(item => item === cardId)
      },
      cardId (name, suit) {
        return `${name}-${suit}`
      },
      select (cardId) {
        this.selected.push(cardId)
      },
      unSelect (cardId) {
        this.selected.splice(this.selected.findIndex(item => item === cardId), 1)
      },
      shuffle() {
        this.cards.sort(() => Math.random() - 0.5);
      },
      suits() {
        this.cards.sort((a, b) => (a.suit > b.suit) ? 1 : (a.suit === b.suit) ? ((a.value > b.value) ? 1 : -1) : -1 )
      },
      value () {
        this.cards.sort((a, b) => (a.value < b.value) ? 1 : -1)
      },
      cardColor (card) {
        return mapping[card]
      }
    }
  }
</script>

<style lang="scss" scoped>
.wrapper {
  padding: 40px 40px 40px 0;
  flex-grow: 1;
  .title {
    font-size: 36px;
    text-align: center;
  }
  .desc {
    font-size: 20px;
    text-align: center;
  }
  .data {
    font-size: 20px;
  }
  .cards {
    margin-top: 30px;
    display: flex;
    align-items: center;
    justify-content: space-between;
    flex-wrap: wrap;
    overflow: scroll;
    max-height: 550px;
    .card {
      margin: 0 10px 10px 0;
      max-width: 200px;
      padding: 20px 40px;
      display: flex;
      flex-direction: column;
      border-radius: 5px;
      align-items: center;
      &.black {
        background-color: #000;
      }
      &.red {
        background-color: #db362a;
      }
      img {
        max-width: 150px;
        margin-bottom: 10px;
      }
      .card-name {
        color: #fff;
        text-align: center;
        margin-bottom: 10px;
        font-size: 18px;
      }
      .button {
        padding: 5px 10px;
        border-radius: 20px;
        width: 100px;
        text-align: center;
        margin-bottom: 10px;
        cursor: pointer;
        color: #3b3b3b;
        background: linear-gradient(to right, #19947c 50%, white 50%);
        background-size: 200% 100%;
        background-position: right bottom;
        transition: all .2s ease-out;
      }

      .button:hover {
        color: #fff;
        background-position: left bottom;
      }
    }
  }
}
</style>