<template>
  <v-app>
    <v-app-bar app>
      <v-toolbar-title class="headline text-uppercase">
        <span>API VK EXAMPLE</span>
      </v-toolbar-title>
      <v-spacer></v-spacer>
      <span></span>
      <v-btn text>
        <span class="mr-2">Login</span>
      </v-btn>
      <v-btn text>
        <span class="mr-2">Logout</span>
      </v-btn>
    </v-app-bar>

    <v-content>
      <v-item-group>
        <v-container grid-list-md>
          <h2 class="font-weight-regular font-italic">6 friends of Ivan Ivanov</h2>
          <v-layout wrap>
            <v-flex v-for="n in 6" :key="n" xs12 md4>
              <v-item>
                <v-card class="flex">
                <v-card-title>Ivan petrov</v-card-title>
                  <v-img
                    :lazy-src="`https://picsum.photos/10/6?image=${n * 5 + 10}`"
                    aspect-ratio="1"
                    class="grey lighten-2"
                  >
                  </v-img>
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
export default {
  name: "App",
  components: {},
  data: () => ({})
};
</script>
