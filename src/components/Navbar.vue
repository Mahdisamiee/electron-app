<template>
    <nav>
        <v-app-bar app clipped-right flat class="red darken-1" dark>
            <v-app-bar-nav-icon @click="drawer = !drawer"></v-app-bar-nav-icon>

            <v-toolbar-title>Firewall</v-toolbar-title>

            <v-spacer></v-spacer>

            <v-layout
                align-center
                style="max-width: 350px"
            >
                <v-text-field
                placeholder="Search..."
                single-line
                color="white"
                hide-details
                ></v-text-field>
                <v-btn small="" flat icon class="mx-3">
                  <v-icon class="text--darken-1" small>mdi-magnify</v-icon>
                </v-btn>
            </v-layout>
        </v-app-bar>
        <v-navigation-drawer right app clipped dark v-model="drawer">
          <v-list>

            <!-- for item that havent sub menu -->
            <v-list-item v-for="item in itemWithoutSub" :key="item" router :to="item.route">
              <v-list-item-icon>
                <v-icon v-text="item.icon"></v-icon>
              </v-list-item-icon>
              <v-list-item-title>{{item.text}}</v-list-item-title>
            </v-list-item>

            <v-list-group
              v-for="item in itemWithSub"
              :key="item"
              :prepend-icon="icon"
              no-action
            >
              <template v-slot:activator>
                <v-list-item-icon>
                  <v-icon v-text="item.icon"></v-icon>
                </v-list-item-icon>
                <v-list-item-content>
                  <v-list-item-title>
                    {{item.text}}
                  </v-list-item-title>
                </v-list-item-content>
              </template>

              <v-list-item v-for="subitem in item.subitems" :key="subitem" router :to="subitem.route">
                <v-list-item-icon>
                  <v-icon v-text="subitem.icon"></v-icon>
                </v-list-item-icon>
                <v-list-item-title>{{subitem.text}}</v-list-item-title>
              </v-list-item>
            </v-list-group>
          </v-list>
        </v-navigation-drawer>
    </nav>
</template>

<script>
export default {
    data(){
        return{
            drawer: null,
            
            items: [
                { icon: 'mdi-home', text: 'خانه', route: '/' },
                { icon: 'mdi-chart-line', text: 'داده ها', route: '/data' },
                { icon: 'mdi-email', text: 'پیام ها', route: '/message' },
                { icon: 'mdi-magnify', text: 'منو تست', route: '/testing', subitems: [
                                                                                        { icon: 'mdi-home', text: 'خانه', route: '/' },
                                                                                        { icon: 'mdi-chart-line', text: 'something', route: '/data' },
                                                                                        { icon: 'mdi-history', text: 'something else' },
                                                                                        ]},
                
            ],
            value: null
        }
    },
    methods:{
        
    },
    computed: {
      itemWithSub() {
        let item = this.items.filter(c => c.hasOwnProperty('subitems'))
        return item
      },
      itemWithoutSub() {
        let item = this.items.filter(c => !c.hasOwnProperty('subitems'))
        return item
      }
    }
}
</script>

<style>

</style>
