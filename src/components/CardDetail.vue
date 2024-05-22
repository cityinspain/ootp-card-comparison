<template>
  <div id="card-detail-container">
    <div id="card-detail" v-if="detailedCard">
      <h1>{{ detailedCard.card_title }}</h1>
      <div id="card-detail-sections">
        <div class="card-detail-section">
          <div>Contact: <RatingBar :value="detailedCard.contact"></RatingBar></div>
          <div>BABIP: <RatingBar :value="detailedCard.babip"></RatingBar></div>
          <div>Avoid K: <RatingBar :value="detailedCard.avoid_ks"></RatingBar></div>
          <div>Gap: <RatingBar :value="detailedCard.gap"></RatingBar></div>
          <div>Power: <RatingBar :value="detailedCard.power"></RatingBar></div>
          <div>Eye: <RatingBar :value="detailedCard.eye"></RatingBar></div>
        </div>
        <div class="card-detail-section">
          <div>Overall: {{ detailedCard.card_value }}</div>
          <div>Batted Ball Profile: {{ detailedCard.batted_ball_type }}</div>
          <div>Groundball Tendency: {{ detailedCard.gb_hitter_type }}</div>
          <div>Flyball Tendency: {{ detailedCard.fb_hitter_type }}</div>
          <div>Speed: <RatingBar :value="detailedCard.speed"></RatingBar></div>
          <div>
            Stealing Aggressiveness: <RatingBar :value="detailedCard.steal_rate"></RatingBar>
          </div>
          <div>Stealing Ability: <RatingBar :value="detailedCard.stealing"></RatingBar></div>
          <div>Baserunning: <RatingBar :value="detailedCard.baserunning"></RatingBar></div>
          <div>Sacrifice Bunt: <RatingBar :value="detailedCard.sac_bunt"></RatingBar></div>
          <div>Bunt for Hit: <RatingBar :value="detailedCard.bunt_for_hit"></RatingBar></div>
        </div>
        <div class="card-detail-section">
          <div style="margin-bottom: 20px">
            <div v-if="detailedCard.pos_rating_catcher">
              Catcher: <RatingBar :value="detailedCard.pos_rating_catcher" />
            </div>
            <div v-if="detailedCard.pos_rating_first_base">
              First Base: <RatingBar :value="detailedCard.pos_rating_first_base" />
            </div>
            <div v-if="detailedCard.pos_rating_second_base">
              Second Base: <RatingBar :value="detailedCard.pos_rating_second_base" />
            </div>
            <div v-if="detailedCard.pos_rating_third_base">
              Third Base: <RatingBar :value="detailedCard.pos_rating_third_base" />
            </div>
            <div v-if="detailedCard.pos_rating_shortstop">
              Shortstop: <RatingBar :value="detailedCard.pos_rating_shortstop" />
            </div>
            <div v-if="detailedCard.pos_rating_left_field">
              Left Field: <RatingBar :value="detailedCard.pos_rating_left_field" />
            </div>
            <div v-if="detailedCard.pos_rating_center_field">
              Center Field: <RatingBar :value="detailedCard.pos_rating_center_field" />
            </div>
            <div v-if="detailedCard.pos_rating_right_field">
              Right Field: <RatingBar :value="detailedCard.pos_rating_right_field" />
            </div>
            <div v-if="detailedCard.pos_rating_pitcher">
              Pitcher: <RatingBar :value="detailedCard.pos_rating_pitcher" />
            </div>
          </div>
          <div>
            <div v-if="detailedCard.position === 2">
              <div>Catcher Blocking: <RatingBar :value="detailedCard.catcher_ability" /></div>
              <div>Catcher Framing: <RatingBar :value="detailedCard.catcher_frame" /></div>
              <div>Catcher Arm: <RatingBar :value="detailedCard.catcher_arm" /></div>
            </div>
            <div v-else-if="detailedCard.position < 7">
              <div>Infield Range: <RatingBar :value="detailedCard.infield_range" /></div>
              <div>Infield Error: <RatingBar :value="detailedCard.infield_error" /></div>
              <div>Infield Arm: <RatingBar :value="detailedCard.infield_arm" /></div>
              <div>Turn DP: <RatingBar :value="detailedCard.double_play" /></div>
            </div>
            <div v-else>
              <div>Outfield Range: <RatingBar :value="detailedCard.outfield_range" /></div>
              <div>Outfield Error: <RatingBar :value="detailedCard.outfield_error" /></div>
              <div>Outfield Arm: <RatingBar :value="detailedCard.outfield_arm" /></div>
            </div>
          </div>
        </div>
      </div>
    </div>
    <div v-else>
      <p>Select a card to see details.</p>
    </div>
  </div>
</template>

<script lang="ts">
import axios from 'axios'
import RatingBar from '@/components/RatingBar.vue'
import { defineComponent, type PropType, watch, ref } from 'vue'

interface Card {
  card_id: number
  card_title: string
  // Add other properties as needed
}

interface DetailedCard extends Card {
  card_value: number
  card_type: number
  card_sub_type: number
  year: number
  peak: string
  team: string
  last_name: string
  first_name: string
  nickname: string | null
  bats: string
  throws: string
  position: number
  pitcher_role: string
  contact: number
  gap: number
  power: number
  eye: number
  avoid_ks: number
  babip: number
  contact_vs_left: number
  gap_vs_left: number
  power_vs_left: number
  eye_vs_left: number
  avoid_k_vs_left: number
  babip_vs_left: number
  contact_vs_right: number
  gap_vs_right: number
  power_vs_right: number
  eye_vs_right: number
  avoid_k_vs_right: number
  babip_vs_right: number
  gb_hitter_type: number
  fb_hitter_type: number
  batted_ball_type: number
  speed: number
  steal_rate: number
  stealing: number
  baserunning: number
  sac_bunt: number
  bunt_for_hit: number
  stuff: number
  movement: number
  control: number
  phr: number
  pbabip: number
  stuff_vs_left: number
  movement_vs_left: number
  control_vs_left: number
  phr_vs_left: number
  pbabip_vs_left: number
  stuff_vs_right: number
  movement_vs_right: number
  control_vs_right: number
  phr_vs_right: number
  pbabip_vs_right: number
  fastball: number
  slider: number
  curveball: number
  changeup: number
  cutter: number
  sinker: number
  splitter: number
  forkball: number
  screwball: number
  circlechange: number
  knucklecurve: number
  knuckleball: number
  stamina: number
  hold: number
  ground_ball: number
  velocity: string
  arm_slot: string
  height: number
  infield_range: number
  infield_error: number
  infield_arm: number
  double_play: number
  catcher_ability: number
  catcher_frame: number
  catcher_arm: number
  outfield_range: number
  outfield_error: number
  outfield_arm: number
  pos_rating_pitcher: number
  pos_rating_catcher: number
  pos_rating_first_base: number
  pos_rating_second_base: number
  pos_rating_third_base: number
  pos_rating_shortstop: number
  pos_rating_left_field: number
  pos_rating_center_field: number
  pos_rating_right_field: number
  learn_catcher: number
  learn_first_base: number
  learn_second_base: number
  learn_third_base: number
  learn_shortstop: number
  learn_left_field: number
  learn_center_field: number
  learn_right_field: number
  era: number
  tier: number
  bref_id: string
  ground_ball_type: string
}

export default defineComponent({
  props: {
    card: {
      type: Object as PropType<Card>,
      required: false
    }
  },
  components: {
    RatingBar
  },
  setup(props) {
    const detailedCard = ref<DetailedCard | null>(null)

    watch(
      () => props.card,
      (newCard, oldCard) => {
        if (newCard && newCard.card_id !== oldCard?.card_id) {
          fetchCardDetails(newCard.card_id)
        }
      },
      { immediate: true }
    )

    function fetchCardDetails(cardId: number) {
      axios
        .get(`http://localhost:3000/cards/${cardId}`)
        .then((response) => {
          detailedCard.value = response.data
        })
        .catch((error) => {
          console.error('Failed to fetch card details:', error)
        })
    }

    return { detailedCard }
  }
})
</script>

<style>
#card-detail {
  display: flex;
  flex-direction: column;
}

#card-detail-sections {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr;
  flex-grow: 1;
  gap: 10px;
}

.card-detail-section {
  display: flex;
  flex-direction: column;
  gap: 5px;
}

#card-detail-container {
  padding-left: 5rem;
}
</style>
