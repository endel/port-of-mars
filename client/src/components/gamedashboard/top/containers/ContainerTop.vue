<template>
  <div class="container-top tour-container-top">
    <ContainerUpkeep />
    <ContainerPhase />
  </div>
</template>

<script lang="ts">
import { Vue, Component } from 'vue-property-decorator';
import ContainerUpkeep from '@port-of-mars/client/components/gamedashboard/top/containers/ContainerUpkeep.vue';
import ContainerPhase from '@port-of-mars/client/components/gamedashboard/top/containers/ContainerPhase.vue';

@Component({
  components: {
    ContainerUpkeep,
    ContainerPhase
  }
})
export default class ContainerTop extends Vue {}
</script>

<style lang="scss" scoped>
@import '@port-of-mars/client/stylesheets/gamedashboard/top/containers/ContainerTop.scss';
</style>
