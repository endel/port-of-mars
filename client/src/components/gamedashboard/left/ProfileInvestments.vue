<template>
  <div class="profile-investments tour-profile-investments">
    <p class="title">Your Investments</p>
    <div class="pi-container">
      <div
        v-for="investment in investments"
        :key="investment.name"
        class="profile-investment"
        v-bind:class="{
          'pending-is-active': investment.pendingUnits > 0,
          'pending-is-inactive': investment.pendingUnits == 0,
          'locked-from-trade': investment.pendingUnits < 0
        }"
        v-b-tooltip.hover="investment.name"
      >
        <div class="main">
          <img
            :src="require(`@port-of-mars/client/assets/icons/${investment.name}.svg`)"
            alt="Investment"
          />
          <!-- <p class="cost">{{ formatCost(investment.cost) }}</p> -->
          <p>
          {{ investment.units}}
          <sup v-show="investment.pendingUnits > 0">
            +
            <span>{{ investment.pendingUnits }}</span>
          </sup>
          <sup v-show="investment.pendingUnits < 0">
            -
            <span>{{ investment.pendingUnits*-1 }}</span>
          </sup>
        </p>
        </div>
        <div class="cost">
          <p>
            Cost <span>{{ formatCost(investment.cost) }}</span>
          </p>
        </div>
      </div>
      <div
        class="profile-investment"
        v-bind:class="{
          'pending-is-active': upkeep.pendingUnits > 0,
          'pending-is-inactive': upkeep.pendingUnits <= 0
        }"
        v-b-tooltip.hover="'system health'"
      >
        <div class="main">
          <img :src="require(`@port-of-mars/client/assets/icons/upkeep.svg`)" alt="Investment" />
          <p>
            {{ upkeep.units
            }}<sup v-show="upkeep.pendingUnits > 0">
              +<span>{{ upkeep.pendingUnits }}</span></sup
            >
          </p>
        </div>
        <div class="cost">
          <p>
            Cost <span>{{ formatCost(upkeep.cost) }}</span>
          </p>
        </div>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { Vue, Component } from 'vue-property-decorator';
import { Resource, RESOURCES, Phase } from '@port-of-mars/shared/types';

@Component({})
export default class ProfileInvestments extends Vue {
  count = 0;
  get gamePhase() {
    return this.$store.state.phase;
  }

  get phases() {
    return Phase;
  }

  get investments() {
    const p = this.$tstore.getters.player;
    const inventory = p.inventory;
    const pendingInventory = p.pendingInvestments;
    const costs = p.costs;
    return RESOURCES.map(name => ({
      name,
      units: inventory[name as Resource],
      pendingUnits: pendingInventory[name as Resource],
      cost: costs[name as Resource]
    }));
  }

  get upkeep() {
    const p = this.$tstore.getters.player;
    const pendingInvestment = p.pendingInvestments;
    const costs = p.costs;
    return {
      pendingUnits: pendingInvestment.upkeep,
      units: p.contributedUpkeep,
      cost: costs.upkeep
    };
  }

  private formatCost(cost: number): number | string {
    return cost !== Number.MAX_SAFE_INTEGER ? cost : '-';
  }

  style(inventory: { units: number; pendingUnits: number }): string {
    return inventory.units < inventory.units + inventory.pendingUnits
      ? 'color: var(--color-Upkeep-opaque-2)'
      : '';
  }
}
</script>

<style lang="scss" scoped>
@import '@port-of-mars/client/stylesheets/gamedashboard/left/ProfileInvestments.scss';

.pending-is-active {
  background-color: $color-Upkeep-opaque-2;
}

.pending-is-inactive {
  background-color: $space-white-opaque-1;
}

.locked-from-trade {
  background-color: $status-red;
}
</style>



