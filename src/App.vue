<template>
  <v-app>
    <v-app-bar app>
      <v-toolbar-title class="headline text-uppercase">
        <span>API VK</span>
      </v-toolbar-title>
      <v-spacer></v-spacer>
      <span></span>
      <v-btn text v-if="!isLogged" @click="login()">
        <span class="mr-2">Авторизоваться</span>
      </v-btn>
      <v-btn text v-if="isLogged" @click="logout()">
        <span class="mr-2">Выход</span>
      </v-btn>
    </v-app-bar>

    <v-content>
      <v-item-group>
        <v-container grid-list-md>
          <div class="text-center headline">
            
            <b>{{ this.isLogged==true ? 'Здравствуйте, '+this.user : 'Залогиньтесь, пожалуйста' }}</b>
          </div>

          <v-layout justify-center wrap>
            <v-flex v-for="item in this.friends" :key="item.id" xs12 md2>
              <v-item>
                <v-card class="flex">
                  <v-card-title>
                    {{ item.first_name }}
                    <br />
                    {{ item.last_name }}
                  </v-card-title>
                  <a :href="`http://vk.com/id${item.id}`">
                    <v-img :src="item.photo_100" aspect-ratio="2" class="grey lighten-2"></v-img>
                  </a>
                  <v-card-text>
                    <v-chip color="success" v-if="item.online==1">online</v-chip>
                    <v-chip v-if="item.online==0">offline</v-chip>
                  </v-card-text>
                </v-card>
              </v-item>
            </v-flex>
          </v-layout>
        </v-container>
      </v-item-group>
    </v-content>
  </v-app>
</template>

<script>
VK.init({
  apiId: 7076590
});

export default {
  name: "App",

  data() {
    return {
      isLogged: null,
      user: "",
      friends: []
    };
  },

  mounted() {
    VK.Auth.getLoginStatus(response => {
      if (response.status === "connected") {
        const user = response.session;
        this.isLogged = true;
        this.getFriends();
        // console.log(this.friends);
        VK.Api.call(
          "users.get",
          {
            v: "5.101",
            fields: ""
          },
          (response) => {
            this.user = `${response.response[0].first_name}  ${response.response[0].last_name}`;
          }
        );
      } else {
        console.log("not authorized");
      }
    });
  },
  methods: {
    login() {
      this.vkAuth();
    },
    logout() {
      VK.Auth.logout(() => {
        this.isLogged = false;
        this.friends = [];
        this.user = "";
      });
    },
    vkAuth() {
      const a = VK.Auth.login(response => {
        if (response.status === "connected") {
          const user = response.session.user;
          this.user = `${user.first_name}  ${user.last_name}`;
          this.isLogged = true;
        }
        this.getFriends();
      }, VK.access.FRIENDS);
    },
    getFriends() {
      VK.Api.call("friends.get", { v: "5.101", fields: "photo_100" }, data => {
        const count = data.response.count;
        const friendsList = data.response.items;
        if (count === 0) {
          console.log("You haven't friends yet");
        } else if (data.response.count < 6) {
          this.friends = friendsList;
        } else {
          for (let i = 0; i < 5; i++) {
            const index = Math.floor(Math.random() * (friendsList.length - 0));
            this.friends.push(friendsList[index]);
            friendsList.splice(index, 1);
          }
        }
      });
    }
  }
};
</script>
