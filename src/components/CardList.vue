<template>
  <div>
    <ul id="card-ul">
      <li v-for="card in cards" :key="card.card_id" @click="selectCard(card)">
        <div class="list-item-container">
          <div class="item-label">
            <span>{{ card.card_type }}</span>
            <br />
            <span>{{ formatCardTitle(card) }}</span>
          </div>
          <div
            class="card-tier"
            :class="{
              'card-tier-iron': card.card_value < 60,
              'card-tier-bronze': card.card_value >= 60 && card.card_value < 70,
              'card-tier-silver': card.card_value >= 70 && card.card_value < 80,
              'card-tier-gold': card.card_value >= 80 && card.card_value < 90,
              'card-tier-diamond': card.card_value >= 90 && card.card_value < 100,
              'card-tier-perfect': card.card_value === 100
            }"
          >
            {{ card.card_value }}
          </div>
        </div>
      </li>
    </ul>
  </div>
</template>

<script lang="ts">
import axios from 'axios'
import { defineComponent } from 'vue'

interface Card {
  card_id: number
  card_title: string
  card_type: string
  card_value: number
  position: number
  first_name: string
  last_name: string
  pitcher_role: string | null
  year: any
}

export default defineComponent({
  emits: ['select-card'],
  data() {
    return {
      cards: [] as Card[]
    }
  },
  mounted() {
    this.fetchCards()
  },
  methods: {
    fetchCards() {
      axios
        .get('http://localhost:3000/cards')
        .then((response) => {
          this.cards = response.data
        })
        .catch((error) => {
          console.error('There was an error!', error)
        })
    },
    selectCard(card: Card) {
      console.log('select card', card)
      this.$emit('select-card', card)
    },
    formatCardTitle(card: Card) {
      // const positions = {
      //     2: "C",
      //     3: "1B",
      //     4: "2B",
      //     5: "3B",
      //     6: "SS",
      //     7: "LF",
      //     8: "CF",
      //     9: "RF",
      //     10: "DH?"
      // }

      const positionMap = new Map([
        [2, 'C'],
        [3, '1B'],
        [4, '2B'],
        [5, '3B'],
        [6, 'SS'],
        [7, 'LF'],
        [8, 'CF'],
        [9, 'RF'],
        [10, 'DH']
      ])

      if (card.position === 1) {
        return `${card.pitcher_role} ${card.first_name} ${card.last_name}`
      }

      return `${positionMap.get(card.position)} ${card.first_name} ${card.last_name}`
    }
  }
})
</script>

<style>
#card-ul {
  font-size: 14pt;
  list-style: none;
  padding-left: 0px;
  max-height: 100vh;
  overflow-y: scroll;
}

.list-item-container {
  border-bottom: 1px solid black;
  height: 50px;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: space-between;
  overflow: hidden;
  /* flex-direction: column; */
}

.list-item-container:hover {
  background-color: #202020;
}

.item-label {
  flex-grow: 1;
  overflow: hidden;
}

.card-tier {
  width: 50px;
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100%;
  color: white;
  font-size: 24px;
  text-align: center;
}

.card-tier-iron {
  background-color: #757575;
}

.card-tier-bronze {
  background-color: #cd7f32;
}
.card-tier-silver {
  background-color: #a8a9ad;
}
.card-tier-gold {
  background-color: #ffc300;
}
.card-tier-diamond {
  background-color: #00d0bc;
}
.card-tier-perfect {
  background-color: black;
}
</style>
