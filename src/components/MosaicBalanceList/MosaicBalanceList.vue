<template>
  <div>
    <Tabs v-if="!isEditionMode" size="small">
      <TabPane :label="$t('assets')" name="name1">
        <img
          class="asset_list pointer"
          src="@/views/resources/img/monitor/monitorAssetList.png"
          @click="isEditionMode = true"
        >
        <div class="mosaicList secondary_page_animate">
          <div
            v-for="(entry, index) in filteredBalanceEntries"
            :key="index"
            class="mosaic_data text_select"
          >
            <span class="img_container">
              <img v-if="entry.id.equals(networkMosaic)"
                src="@/views/resources/img/symbol/XYMCoin.png" alt />
              <img v-else src="@/views/resources/img/symbol/XYMCoin.png" class="grayed-xym-logo"/>
            </span>
            <span class="mosaic_name">{{ entry.name !== '' ? entry.name : entry.id.toHex() }}</span>
            <span class="mosaic_value">
              <MosaicAmountDisplay :id="entry.id" :relative-amount="entry.amount" :size="'normal'" />
            </span>
          </div>
        </div>
      </TabPane>
    </Tabs>
    <div v-else class="searchMosaic secondary_page_animate">
      <img
        src="@/views/resources/img/monitor/monitorLeftArrow.png"
        class="asset_setting_tit pointer"
        alt
        @click="isEditionMode = false"
      >
      <div class="mosaicList">
        <div class="toggle_all_checked">
          <span @click="toggleMosaicDisplay()">
            <div :class="[ 'choose', formItems.hasCheckedAll ? 'true' : 'false' ]" />
            {{ !formItems.hasCheckedAll ? $t('select_all') : $t('all_unchecked') }}
          </span>
          <span @click="return false">
            <div :class="[ 'choose', formItems.hasShowExpired ? 'true' : 'false' ]" />
            {{ $t('Display_expired_mosaic') }}
          </span>
        </div>

        <div
          v-for="(entry, index) in filteredBalanceEntries"
          :key="index"
          :class="[ 'mosaic_data',index === 0 ? 'padding_top_0' : '' ]"
          class="mosaic_data pointer text_select"
          @click="toggleMosaicDisplay(entry.mosaic.id)"
        >
          <!-- @TODO: Mosaic list actions not working -->
          <span class="namege_img">
            <img
              class="small_icon"
              :src="hasHiddenMosaic(entry.mosaic.id)
                ? dashboardImages.monitorUnselected : dashboardImages.monitorSelected"
            >
              <img v-if="entry.id.equals(networkMosaic)"
                src="@/views/resources/img/symbol/XYMCoin.png" class="mosaicIcon" alt />
              <img v-else src="@/views/resources/img/symbol/XYMCoin.png" class="mosaicIcon grayed-xym-logo"/>
          </span>
          <span class="mosaic_name text_select">
            {{ entry.name !== '' ? entry.name : entry.id.toHex() }}
          </span>
          <span class="mosaic_value">
            <MosaicAmountDisplay :id="entry.id" :relative-amount="entry.amount" :size="'normal'" />
          </span>
        </div>
        <div class="complete_container">
          <div class="complete" @click="isEditionMode = false">
            {{ $t('Close') }}
          </div>
        </div>
        <div class="mosaic_data" />
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { MosaicBalanceListTs } from './MosaicBalanceListTs'
import './MosaicBalanceList.less'

export default class MosaicBalanceList extends MosaicBalanceListTs {}
</script>
