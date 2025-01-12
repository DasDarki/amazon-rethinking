<script setup lang="ts">
import {useRoute} from "vue-router";
import {computed, ref, watch} from "vue";
import Frame from "@/components/Frame.vue";

const route = useRoute();
const product = computed(() => route.query.p as string);
const bestOfferIsNotAvailable = computed(() => route.query.b === 'false');

const includeOffers = ref<'just_amazon'|'all'>(localStorage.getItem('includeOffers') as 'just_amazon'|'all' || 'just_amazon');
watch(() => includeOffers.value, () => localStorage.setItem('includeOffers', includeOffers.value));

const sortBy = ref<'amazon'|'send'|'price'>('amazon');
const selectedOffer = ref<any>();

watch(() => sortBy.value, () => selectedOffer.value = undefined);

const offers = ref([
  {
    price: 119.99,
    sendIn: 4,
    new: true,
    official: true,
    available: !bestOfferIsNotAvailable.value
  },
  {
    price: 99.99,
    sendIn: 3,
    new: true,
    official: false,
    available: true
  },
  {
    price: 89.99,
    sendIn: 7,
    new: false,
    official: true,
    available: true
  },
  {
    price: 79.99,
    sendIn: 10,
    new: false,
    official: false,
    available: true
  }
]);

const sortedOffers = computed(() => {
  const offersList = offers.value.filter(o => o.available);

  if (sortBy.value === 'amazon') {
    return offersList.sort((a, b) => {
      if (a.official && !b.official) return -1;
      if (!a.official && b.official) return 1;
      if (a.new && !b.new) return -1;
      if (!a.new && b.new) return 1;
      return 0;
    });
  }

  if (sortBy.value === 'send') {
    return offersList.sort((a, b) => a.sendIn - b.sendIn);
  }

  if (sortBy.value === 'price') {
    return offersList.sort((a, b) => a.price - b.price);
  }

  return offersList;
});

const displayedOffer = computed(() => {
  if (!selectedOffer.value) return sortedOffers.value[0];
  return selectedOffer.value;
});

const betterAvailableOffer = computed(() => {
  let offer = displayedOffer.value;
  let dirty = false;

  if (sortBy.value === 'amazon' || sortBy.value === 'price') {
    for (let i = 0; i < sortedOffers.value.length; i++) {
      if (sortedOffers.value[i].price < offer.price) {
        offer = sortedOffers.value[i];
        dirty = true;
      }
    }
  } else if (sortBy.value === 'send') {
    for (let i = 0; i < sortedOffers.value.length; i++) {
      if (sortedOffers.value[i].sendIn < offer.sendIn) {
        offer = sortedOffers.value[i];
        dirty = true;
      }
    }
  }

  return dirty ? offer : null;
});
</script>

<template>
  <div style="width: 100%; display: flex; justify-content: center; margin-top: 1rem">
    <div style="display: flex; flex-direction: row; width: 60%; padding-top: 1rem; padding-bottom: 1rem; gap: 2.5rem">
      <Frame text="Product Images" height="20" style="width: 30rem"/>

      <div style="display: flex; flex-direction: column">
        <h1>{{product}}</h1>
        <Frame text="Product Details" height="40" style="width: 30rem"/>
      </div>

      <div style="display: flex; flex-direction: column; border-radius: 0.5rem; border: 1px solid rgba(0, 0, 0, 0.2); width: 15rem; padding: 0.5rem 1rem">
        <h3>
          {{displayedOffer.price}}€
        </h3>
        <span>
          send in {{displayedOffer.sendIn}} days | {{displayedOffer.new ? 'New' : 'Used'}}
        </span>
        <span v-if="displayedOffer.official" style="color: #F3A847; font-weight: bold; text-decoration: underline; font-style: italic; margin-top: 0.5rem">
          Official from Amazon
        </span>

        <template v-if="betterAvailableOffer">
          <hr style="margin: 1rem 0"/>

          <div style="display: flex; flex-direction: column">
            <button class="button" @click="selectedOffer = betterAvailableOffer">
              There is a better offer available, click here to select it
            </button>

            <span v-if="!betterAvailableOffer.official" style="color: red; text-decoration: underline; font-style: italic; margin-top: 0.5rem">
            Not official from Amazon
          </span>
          </div>
        </template>

        <hr style="margin: 1rem 0"/>

        <select v-model="sortBy" style="margin-bottom: 1rem">
          <option value="amazon">Sort Offer by: Official</option>
          <option value="send">Sort Offer by: Send Days</option>
          <option value="price">Sort Offer by: Price</option>
        </select>


        <hr style="margin: 1rem 0"/>
        <select v-model="includeOffers" style="margin-bottom: 1rem">
          <option value="just_amazon">Show Offers: Just Amazon</option>
          <option value="all">Show Offers: All</option>
        </select>

        <div v-for="offer in sortedOffers.filter(o => (includeOffers === 'all' || o.official) && displayedOffer !== o)" style="display: flex; flex-direction: column; border-radius: 0.5rem; border: 1px solid rgba(0, 0, 0, 0.2); padding: 0.5rem 1rem; margin-top: 0.5rem">
          <h3>
            {{offer.price}}€
          </h3>
          <span>
              send in {{offer.sendIn}} days | {{offer.new ? 'New' : 'Used'}}
            </span>
          <span v-if="offer.official" style="color: #F3A847; font-weight: bold; text-decoration: underline; font-style: italic; margin-top: 0.5rem">
              Official from Amazon
            </span>

          <button class="button" @click="selectedOffer = offer" style="margin-top: 0.5rem">
            Select this offer
          </button>
        </div>
      </div>
    </div>
  </div>
</template>