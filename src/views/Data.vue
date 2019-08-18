<template>
    <v-container class="blue-grey darken-4 pa-4 my-5" style="maxWidth:80%;" grid-list-xs>
        <v-layout row wrap>
            <v-flex xs12 class="my-5">
                <v-layout row>
                    <v-flex class="text-center my-5 white--text" xs12>
                        <span class="title mx-3">Data and Progress</span>
                        <span class="caption"> last week</span>
                        <v-divider dark></v-divider>
                    </v-flex>
                    <v-flex xs12>
                        <v-layout row wrap class="grey darken-3 pa-3 white--text">
                            <v-flex xs12 sm6 my-3 v-for="(value, i) in values" :key="i" class="text-center">
                                <v-card outlined class="mx-auto text-center grey--text" max-width="400px">
                                    <v-card-title class="justify-center" primary-title>
                                        : Data {{i+1}}
                                    </v-card-title>
                                    <v-card-text>
                                        <v-progress-circular
                                            :rotate="180"
                                            :size="100"
                                            :width="15"
                                            :value="value.value"
                                            :color="value.color"
                                        >
                                            {{value.value}}
                                        </v-progress-circular>

                                    </v-card-text>
                                </v-card>
                            </v-flex>
                        </v-layout>
                    </v-flex>
                </v-layout>
            </v-flex>

            <v-flex xs12 class="text-center mt-5 white--text">
                <span class="title">Data Tables</span>
            </v-flex>

            <v-flex xs12 class="my-5" >
                <v-container
                    fluid
                    grid-list-md
                    class="grey darken-2"
                >
                    <v-data-iterator
                    :items="items"
                    :items-per-page.sync="itemsPerPage"
                    :page="page"
                    :search="search"
                    :sort-by="sortBy.toLowerCase()"
                    
                    hide-default-footer
                    >
                        <template v-slot:header>
                            <v-toolbar
                                dark
                                class="grey darken-2 mb-2"
                                flat
                                >
                                <v-text-field
                                    v-model="search"
                                    clearable
                                    flat
                                    solo-inverted
                                    hide-details
                                    prepend-inner-icon="mdi-magnify"
                                    label="Search"
                                ></v-text-field>
                                <template v-if="$vuetify.breakpoint.mdAndUp">
                                    <v-spacer></v-spacer>
                                    <v-select
                                        v-model="sortBy"
                                        flat
                                        solo-inverted
                                        hide-details
                                        :items="keys"
                                        prepend-inner-icon="mdi-magnify"
                                        label="Sort by"
                                        clearable
                                    ></v-select>
                                    <!--<v-spacer></v-spacer>
                                    <v-btn-toggle
                                        v-model="sortDesc"
                                        mandatory
                                    >
                                        <v-btn
                                            large
                                            depressed
                                            color="blue"
                                            :value="false"
                                        >
                                            <v-icon>mdi-menu-down</v-icon>
                                        </v-btn>
                                        <v-btn
                                            large
                                            depressed
                                            color="blue"
                                            :value="true"
                                        >
                                            <v-icon>mdi-menu-up</v-icon>
                                        </v-btn>
                                    </v-btn-toggle> -->
                                </template>
                            </v-toolbar>
                        </template>

                        <template v-slot:default="props">
                            <v-layout
                                wrap
                            >
                                <v-flex
                                    v-for="item in props.items"
                                    :key="item.name"
                                    xs12
                                    sm6
                                    md4
                                    lg3
                                >
                                    <v-card>
                                        <v-card-title class="subheading font-weight-bold">{{ item.name }}</v-card-title>

                                        <v-divider></v-divider>

                                        <v-list dense>
                                            <v-list-item
                                            v-for="(key, index) in filteredKeys"
                                            :key="index"
                                            :color="sortBy === key.toLowerCase() ? `blue lighten-4` : `white`"
                                            >
                                            <v-list-item-content>{{ key }}:</v-list-item-content>
                                            <v-list-item-content class="align-end">{{ item[key.toLowerCase()] }}</v-list-item-content>
                                            </v-list-item>
                                        </v-list>
                                    </v-card>
                                </v-flex>
                            </v-layout>
                        </template>

                        <template v-slot:footer>
                            <v-layout mt-2 wrap align-center justify-center>
                                <span class="grey--text">Items per page</span>
                                <v-menu offset-y>
                                    <template v-slot:activator="{ on }">
                                        <v-btn
                                            dark
                                            text
                                            color="white"
                                            class="ml-2"
                                            v-on="on"
                                        >
                                            {{ itemsPerPage }}
                                            <v-icon>mdi-menu-down</v-icon>
                                        </v-btn>
                                    </template>
                                    <v-list>
                                        <v-list-item
                                            v-for="(number, index) in itemsPerPageArray"
                                            :key="index"
                                            @click="updateItemsPerPage(number)"
                                        >
                                            <v-list-item-title>{{ number }}</v-list-item-title>
                                        </v-list-item>
                                    </v-list>
                                </v-menu>

                                <v-spacer></v-spacer>

                                <span
                                    class="ml-4
                                    grey--text"
                                >
                                    Page {{ page }} of {{ numberOfPages }}
                                </span>
                                <v-btn
                                    fab
                                    dark
                                    color="grey darken-3"
                                    class="ml-1 white--text"
                                    @click="nextPage"
                                >
                                    <v-icon>mdi-menu-right</v-icon>
                                </v-btn>
                                <v-btn
                                    fab
                                    dark
                                    color="grey darken-3"
                                    class="mr-1 white--text"
                                    @click="formerPage"
                                >
                                    <v-icon>mdi-menu-left</v-icon>
                                </v-btn>
                            </v-layout>
                        </template>
                    </v-data-iterator>
                </v-container>
            </v-flex>
            
        </v-layout>
    </v-container>
</template>

<script>
import { clearInterval, setTimeout } from 'timers';
export default {
    data(){
        return{
            value1: 90,
            value2: 60,
            value3: 40,
            value4: 20,
            values: [
                {value : 90, id: 1, color: 'pink'},
                {value : 30, id: 2, color: 'info'},
                {value : 40, id: 3, color: 'orange'},
                {value : 60, id: 4, color: 'teal'},
            ],
            interval: {},

            //------------
            itemsPerPageArray: [4, 8, 12],
            search: '',
            filter: {},
            sortDesc: false,
            page: 1,
            itemsPerPage: 4,
            sortBy: 'name',
            keys: [
                'Name',
                'Calories',
                'Fat',
                'Carbs',
                'Protein',
                'Sodium',
                'Calcium',
                'Iron',
            ],
            items: [
                {
                    name: 'Frozen Yogurt',
                    calories: 159,
                    fat: 6.0,
                    carbs: 24,
                    protein: 4.0,
                    sodium: 87,
                    calcium: '14%',
                    iron: '1%',
                },
                {
                    name: 'Ice cream sandwich',
                    calories: 237,
                    fat: 9.0,
                    carbs: 37,
                    protein: 4.3,
                    sodium: 129,
                    calcium: '8%',
                    iron: '1%',
                },
                {
                    name: 'Eclair',
                    calories: 262,
                    fat: 16.0,
                    carbs: 23,
                    protein: 6.0,
                    sodium: 337,
                    calcium: '6%',
                    iron: '7%',
                },
                {
                    name: 'Cupcake',
                    calories: 305,
                    fat: 3.7,
                    carbs: 67,
                    protein: 4.3,
                    sodium: 413,
                    calcium: '3%',
                    iron: '8%',
                },
                {
                    name: 'Gingerbread',
                    calories: 356,
                    fat: 16.0,
                    carbs: 49,
                    protein: 3.9,
                    sodium: 327,
                    calcium: '7%',
                    iron: '16%',
                },
                {
                    name: 'Jelly bean',
                    calories: 375,
                    fat: 0.0,
                    carbs: 94,
                    protein: 0.0,
                    sodium: 50,
                    calcium: '0%',
                    iron: '0%',
                },
                {
                    name: 'Lollipop',
                    calories: 392,
                    fat: 0.2,
                    carbs: 98,
                    protein: 0,
                    sodium: 38,
                    calcium: '0%',
                    iron: '2%',
                },
                {
                    name: 'Honeycomb',
                    calories: 408,
                    fat: 3.2,
                    carbs: 87,
                    protein: 6.5,
                    sodium: 562,
                    calcium: '0%',
                    iron: '45%',
                },
                {
                    name: 'Donut',
                    calories: 452,
                    fat: 25.0,
                    carbs: 51,
                    protein: 4.9,
                    sodium: 326,
                    calcium: '2%',
                    iron: '22%',
                },
                {
                    name: 'KitKat',
                    calories: 518,
                    fat: 26.0,
                    carbs: 65,
                    protein: 7,
                    sodium: 54,
                    calcium: '12%',
                    iron: '6%',
                },
            ],
            
        }
    },

    beforeDestroy () {
        clearInterval(this.interval)
    },

    
    computed: {
        numberOfPages () {
            return Math.ceil(this.items.length / this.itemsPerPage)
        },
        filteredKeys () {
            return this.keys.filter(key => key !== `Name`)
        },
    },
    methods: {
        nextPage () {
            if (this.page + 1 <= this.numberOfPages) this.page += 1
        },
        formerPage () {
            if (this.page - 1 >= 1) this.page -= 1
        },
        updateItemsPerPage (number) {
            this.itemsPerPage = number
        },
    },

}
</script>

<style scoped>
.v-progress-circular{
    margin: 1rem;
}
</style>