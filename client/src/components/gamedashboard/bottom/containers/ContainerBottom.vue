<template>
  <div class="container-bottom tour-container-bottom">
    <div class="b-row">
      <div class="view">
        <ContainerInvestments v-if="gamePhase == phase.invest" />
        <!-- SCSS REVISIT -->
        <ContainerEvents v-else-if="gamePhase == phase.events" />
        <ContainerTrade v-else-if="gamePhase == phase.trade" />
        <ContainerPurchase v-else-if="gamePhase == phase.purchase" />
        <ContainerDiscard v-else-if="gamePhase == phase.discard" />
        <ContainerDefault v-else />
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { Vue, Component } from 'vue-property-decorator';
import ContainerEvents from '@port-of-mars/client/components/gamedashboard/bottom/containers/ContainerEvents.vue';
import ContainerInvestments from '@port-of-mars/client/components/gamedashboard/bottom/containers/ContainerInvestments.vue';
import ContainerDefault from '@port-of-mars/client/components/gamedashboard/bottom/containers/ContainerDefault.vue';
import ContainerTrade from '@port-of-mars/client/components/gamedashboard/bottom/containers/ContainerTrade.vue';
import ContainerPurchase from '@port-of-mars/client/components/gamedashboard/bottom/containers/ContainerPurchase.vue';
import ContainerDiscard from '@port-of-mars/client/components/gamedashboard/bottom/containers/ContainerDiscard.vue';
import { Phase } from '@port-of-mars/shared/types';

@Component({
  components: {
    ContainerEvents,
    ContainerTrade,
    ContainerDefault,
    ContainerInvestments,
    ContainerPurchase,
    ContainerDiscard
  }
})
export default class ContainerBottom extends Vue {
  get phase() {
    return Phase;
  }

  get gamePhase() {
    return this.$store.state.phase;
  }
}
</script>

<style lang="scss" scoped>
@import '@port-of-mars/client/stylesheets/gamedashboard/bottom/containers/ContainerBottom.scss';
</style>
