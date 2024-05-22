<template>
  <div class="master-detail">
    <CardList id="card-list" @select-card="handleSelectCard" />
    <CardDetail :card="selectedCard" />
  </div>
</template>

<script lang="ts">
import CardList from './CardList.vue'
import CardDetail from './CardDetail.vue'
import { defineComponent } from 'vue'

interface Card {
  card_id: number
  card_title: string
}

export default defineComponent({
  components: {
    CardList,
    CardDetail
  },
  data() {
    return {
      selectedCard: null as Card | null
    }
  },
  methods: {
    handleSelectCard(card: Card) {
      this.selectedCard = card
    }
  }
})
</script>

<style>
.master-detail {
  display: flex;
}

#card-list {
  min-width: fit-content;
  font-size: 24px;
  padding: 0px;
  margin: 0px;
}
</style>
