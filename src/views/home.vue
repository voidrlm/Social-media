<template>
  <v-container fluid>
    <v-card-title
      :class="
        $vuetify.breakpoint.mdAndUp ? 'text-h4 mt-n7' : 'text-h5 justify-center'
      "
      ><v-icon
        size="35"
        class="mr-2"
        :color="$vuetify.theme.dark ? 'white' : 'black'"
        >{{
          getGreetingData === "Good Evening"
            ? "mdi-moon-waning-crescent"
            : "mdi-white-balance-sunny"
        }}</v-icon
      >{{ getGreetingData }},
      {{ $store.getters.currentUser.name }} !</v-card-title
    >

    <v-layout justify-start class="mt-3">
      <v-slide-group v-model="model" class="pa-0" show-arrows>
        <v-slide-item
          v-for="n in 8"
          :key="n"
          v-slot="{ active, toggle }"
          class="rounded-xl"
        >
          <v-card
            :color="active ? 'primary' : 'grey lighten-1'"
            class="ma-2"
            height="200"
            width="100"
            @click="toggle"
          >
            <v-row class="fill-height" align="center" justify="center">
              <v-scale-transition>
                <v-icon
                  v-if="active"
                  color="white"
                  size="48"
                  v-text="'mdi-close-circle-outline'"
                ></v-icon>
              </v-scale-transition>
            </v-row>
          </v-card>
        </v-slide-item>
      </v-slide-group>
    </v-layout>
    <v-card
      class="mx-auto mt-5"
      color="secondary lighten-3"
      v-for="(post, index) in postsData"
      :key="index"
    >
      <v-tooltip right>
        <template v-slot:activator="{ on, attrs }">
          <v-card-title v-bind="attrs" v-on="on">
            <v-list-item class="grow">
              <v-list-item-avatar color="accent">
                <v-img
                  class="elevation-6"
                  :alt="post.uploadedBy"
                  :src="post.uploaderAvatar"
                ></v-img>
              </v-list-item-avatar>

              <v-list-item-content>
                <v-list-item-title
                  >{{ post.uploadedBy
                  }}<v-icon
                    size="15"
                    class="ml-2"
                    :class="post.isUploaderOnline ? 'green--text' : ''"
                    v-text="'mdi-circle'"
                  ></v-icon
                ></v-list-item-title>
              </v-list-item-content>
            </v-list-item>
          </v-card-title>
        </template>
        <span>Left tooltip</span>
      </v-tooltip>

      <v-card-text class="text-h5 mx-4 font-weight-bold">
        {{ post.text }}
      </v-card-text>
      <v-img
        v-if="post.img"
        class="elevation-0"
        :alt="post.uploadedBy"
        :src="post.img"
        max-height="500"
        contain
      ></v-img>
      <v-card-actions>
        <v-list-item class="grow">
          <v-row align="center" justify="end">
            <v-icon class="mr-1"> mdi-heart </v-icon>
            <span class="subheading mr-2">{{ post.likes }}</span>
            <span class="mr-1">??</span>
            <v-icon class="mr-1"> mdi-share-variant </v-icon>
          </v-row>
        </v-list-item>
      </v-card-actions>
    </v-card>
  </v-container>
</template>
<script>
import { postsData } from "@/resources/postsDatabase";
import { friendsData } from "@/resources/friendsDatabase";
export default {
  name: "home-component",
  data: () => ({ postsData, friendsData, model: null }),
  computed: {
    getGreetingData() {
      var today = new Date();
      var curHr = today.getHours();

      return curHr < 12
        ? "Good Morning"
        : curHr > 18
        ? "Good Evening"
        : "Good Afternoon";
    },
  },
  created() {
    postsData.map((post) => {
      let userObject = friendsData.filter(function (user) {
        return user.userId === post.userId;
      });
      post.uploaderId = userObject[0].userId;
      post.uploadedBy = userObject[0].name;
      post.uploaderAvatar = userObject[0].icon;
      post.isUploaderOnline = userObject[0].isOnline;
    });
  },
  methods: {},
};
</script>
