<template>
  <div id="page" :class="{hideNotSelected}">
    <main>
      <div v-if="selected.length === 0">
        Nothing selected 
      </div>
      <div v-else>
        <div class="share-summary">
          The choices have been made.
        </div>
        <view-deck-flat
          v-if="view === 'flat'"
          :groupData="deckData"
          :hideNotSelected="hideNotSelected"
          :level="1"
        />
        <view-deck-card-group
          v-else
          :groupData="deckData"
          :hideNotSelected="hideNotSelected"
          :level="1"
        />
      </div>
      <modal-box v-if="showShareDetails" @close="showShareDetails = false">
        <h3 slot="header">Share Details</h3>
        <div slot="body"> 
          <span>
            To share simply copy and send the following link:
          </span>
          <input class="share-url" type="text" :value="shareUrl" autofocus>
        </div>
      </modal-box>
    </main>
    <footer>
      <div>
        <button @click="toggleHideNotSelected">
          {{this.hideNotSelected ? 'Show' : 'Hide'}} Unchosen
        </button>
        <button @click="toggleView">
          {{this.view === 'card' ? 'Flat' : 'Card'}} View
        </button>
        <button @click="editChoices">
          Edit Choices
        </button>
      </div>
      <div>
        <button class="success" @click="showShareDetails = true">
          Share Choices
        </button>
      </div>
    </footer>
  </div>  
</template>

<script>
import ViewDeckCardGroup from '@/components/ViewDeckCardGroup.vue'
import ViewDeckFlat from '@/components/ViewDeckFlat.vue'
import ModalBox from '@/components/ModalBox.vue'
import deckMixin from '@/mixins/deck'
export default {
  name: 'Share',
  props: ['hash'],
  mixins: [deckMixin],
  data () {
    return {
      hideNotSelected: true,
      view: 'card',
      showShareDetails: false
    }
  },
  created () {
    if (this.$route.params.hash) {
      this.restoreStatusHash(this.$route.params.hash, this.deckData)
    }
  },
  components: {ViewDeckCardGroup, ViewDeckFlat, ModalBox},
  computed: {
    shareUrl () {
      return window.location.href
    }
  },
  methods: {
    toggleHideNotSelected () {
      this.hideNotSelected = !this.hideNotSelected
    },
    toggleView () {
      this.view = this.view === 'card' ? 'flat' : 'card'
    },
    editChoices () {
      this.$router.push({name: 'Index'})
    }
  }
}
</script>

<style scoped lang="scss">
  @import '../mixins/deck.scss';
  @import '../mixins/ui.scss';
  .share-summary {
    text-align: center;
  }
  .modal-box .share-url {
    display: block;
    width: 100%;
    margin: 0.5em 0;
  }
</style>