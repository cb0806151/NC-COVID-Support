<template>
  <div class="resultWrapper">
    <div ref="results" class="resultList">
      <div
        v-for="(item, index) in filteredMarkers"
        v-bind:key="index"
        class="resultItem"
        :class="{ selected: index == location.locValue, closedOne: item.oc == false }"
        :ref="'result' + index"
        @click="$emit('location-selected', { locValue: index, isSetByMap: false })"
      >
        <h5 class="resultTitle">{{ item.marker.gsx$providername.$t }}</h5>
        <template v-if="!!item.marker.gsx$provideraddloc.$t"
          ><div class="addloc">{{ item.marker.gsx$provideraddloc.$t }}</div></template
        >
        <div v-if="!item.oc" class="closed">Closed on {{ getDay }}</div>
        <span class="resultAddress">
          <span v-if="!!item.marker.gsx$cuisine.$t">{{ item.marker.gsx$cuisine.$t }}<br /></span>
          {{ item.marker.gsx$address.$t }},
          {{ item.marker.gsx$city.$t }}
        </span>
        <template v-if="item.marker.gsx$discountmedical.$t == 1"
          ><span :title="$tc('label.discounts', 1)"><i class="fas fa-user-md" /></span
        ></template>
        <template v-if="item.marker.gsx$familymeal.$t == 1"
          ><span :title="$tc('category.family', 2)"><i class="fas fa-user-friends" /></span
        ></template>
        <template v-if="item.marker.gsx$mealstudent.$t == 1"
          ><span :title="$tc('label.mealsforstudents', 1)"><i class="fas fa-school" /></span
        ></template>
        <template v-if="item.marker.gsx$mealstudent.$t == 1"
          ><span :title="$tc('label.opentopublic', 1)"><i class="fas fa-users" /></span
        ></template>
        <template v-if="item.marker.gsx$drivethru.$t == 1"
          ><span :title="$t('label.drivethru')"><i class="fas fa-car-side" /></span
        ></template>
        <template v-if="item.marker.gsx$curbside.$t == 1"
          ><span :title="$tc('label.curbsidepickup', 1)"><i class="fas fa-car" /></span
        ></template>
        <template v-if="item.marker.gsx$orderonline.$t == 1"
          ><span :title="$t('label.orderonline')"><i class="fas fa-mouse" /></span
        ></template>
        <template v-if="item.marker.gsx$delivery.$t == 1"
          ><span :title="$t('label.delivery')"><i class="fas fa-shipping-fast" /></span
        ></template>
      </div>
    </div>
  </div>
</template>

<script>
import IconListItem from './IconListItem.vue'
import { weekdaysJs } from '../constants'

export default {
  name: 'ResultsList',
  data() {
    return {
      selected: false
    }
  },
  components: {
    IconListItem
  },
  computed: {
    getDay: function () {
      return this.$t(`dayofweek.${weekdaysJs[this.day].day}`)
    }
  },
  props: {
    filteredMarkers: Array,
    location: { locValue: Number, isSetByMap: Boolean },
    day: Number
  },
  watch: {
    location: function (locationVal) {
      if (locationVal.isSetByMap) {
        var top = this.$refs['result' + locationVal.locValue][0].offsetTop - 330
        this.$refs['results'].scrollTo(0, top)
      }
    }
  }
}
</script>

<style>
.addloc {
  margin-bottom: 8px;
}
.resultList {
  max-height: calc(100vh - 294px);
  overflow-y: auto;
}
.resultItem {
  padding: 10px;
  display: block;
  min-height: 100px;
  border-bottom: solid 1px rgba(0, 0, 0, 0.125);
  background: #fff;
  font-size: 0.8rem;
  max-width: 282px;
}
.resultItem:hover {
  background: #f8f9fa !important;
  cursor: pointer;
}

/* .resultItem.selected {
  background: #dffafe !important;
} */

.resultItem a {
  color: #000;
}

.resultItem > span > i {
  margin-right: 8px;
  color: #2eb7cb;
  font-size: 1rem;
  margin-top: 6px;
}

.resultTitle {
  font-size: 0.9rem;
  margin-bottom: 0;
}
.resultAddress {
  font-size: 0.8rem;
  display: block;
  width: 17rem;
}
.closedOne {
  /* background: #f9f9f9 !important; */
}
</style>
