<template>
  <div class="home">
    <v-layout align-center justify-end row fill-height ma-2>
      <div id="user" class="ma-1" v-for="user in team.team.users" :key="user.id" @click="$router.push({ name: 'user', params: {name: user.username} })">
        <avatar-build-gauge :user="user" :merge-requests="userMergeRequests(user.username)"/>
      </div>
    </v-layout>
    <build-gauge :merge-requests="mergeRequests" v-if="mergeRequests.length"/>
    <merge-requests :merge-requests="mergeRequests" v-if="mergeRequests.length"/>
  </div>
</template>

<script>
import { mapGetters, mapState } from "vuex";
import MergeRequests from "@/components/MergeRequests.vue";
import BuildGauge from "@/components/BuildGauge.vue";
import AvatarBuildGauge from "@/components/AvatarBuildGauge.vue";
import UserAvatarPopover from "@/components/UserAvatarPopover.vue";
import store from "@/store";

export default {
  name: "team",
  components: {
    MergeRequests,
    "user-avatar": UserAvatarPopover,
    BuildGauge,
    AvatarBuildGauge
  },
  computed: {
    ...mapState(["team"]),
    ...mapGetters({
      mergeRequests: "getMergeRequests",
      userMergeRequests: "getMergeRequestsForUser"
    })
  },
  beforeRouteEnter(to, from, next) {
    store.dispatch("loadTeam", to.params.name);
    next();
  },
  beforeRouteUpdate(to, from, next) {
    store.dispatch("loadTeam", to.params.name);
    next();
  }
};
</script>
<style lang="stylus" scoped>
  #user {
    cursor: pointer
  }
</style>
