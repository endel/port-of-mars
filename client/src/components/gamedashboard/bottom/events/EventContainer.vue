<template>
  <div class="event-view">
    <component :is="eventView" :eventView="layout"></component>
  </div>
</template>

<script lang="ts">
import { Vue, Component, Prop } from 'vue-property-decorator';
import { MarsEventData, EventClientView } from '@port-of-mars/shared/types';
import EventVote from '@port-of-mars/client/components/gamedashboard/bottom/events/EventVote.vue';
import EventInfluences from '@port-of-mars/client/components/gamedashboard/bottom/events/EventInfluences.vue';
import EventAccomplishments from '@port-of-mars/client/components/gamedashboard/bottom/events/EventAccomplishments.vue';
import EventNoChange from '@port-of-mars/client/components/gamedashboard/bottom/events/EventNoChange.vue';

@Component({
  components: {
    EventVote,
    EventInfluences,
    EventAccomplishments,
    EventNoChange
  }
})
export default class EventContainer extends Vue {
  @Prop() private event!: MarsEventData;

  private eventVoteViews: Array<EventClientView> = [
    'VOTE_YES_NO',
    'VOTE_FOR_PLAYER_SINGLE',
    'VOTE_FOR_PLAYER_HERO_PARIAH'
  ];
  private eventInfluencesViews: Array<EventClientView> = [
    'INFLUENCES_SELECT',
    'INFLUENCES_DRAW'
  ];
  private eventAccomplishmentsViews: Array<EventClientView> = [
    'ACCOMPLISHMENT_SELECT_PURCHASED'
  ];
  private eventNoChangeViews: Array<EventClientView> = [
    'NO_CHANGE',
    'AUDIT',
    'DISABLE_CHAT'
  ];

  get layout() {
    return this.event.clientViewHandler;
  }

  get eventView(): string {
    // TODO: NO_CHANGE VIEW (MAYBE)
    if (this.eventVoteViews.includes(this.layout)) {
      return 'EventVote';
    } else if (this.eventInfluencesViews.includes(this.layout)) {
      return 'EventInfluences';
    } else if (this.eventAccomplishmentsViews.includes(this.layout)) {
      return 'EventAccomplishments';
    } else if (this.eventNoChangeViews.includes(this.layout)) {
      return 'EventNoChange';
    }
    return '';
  }
}
</script>

<style lang="scss" scoped>
@import '~animate.css/source/attention_seekers/pulse.css';
@import '@port-of-mars/client/stylesheets/gamedashboard/bottom/events/EventContainer.scss';
</style>
