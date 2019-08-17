<template>
    <v-container mt-5 grid-list-md style="maxWidth:80%;">


        <div class="pa-2">
            <v-app-bar
                color="blue-grey darken-4"
                dark
                flat
                class="pa-2"
            >
                <v-toolbar-title>Data and information</v-toolbar-title>

                <v-spacer></v-spacer>

                <v-badge
                    color="orange darken-1"
                    overlap
                    left
                    class="ml-3"
                >
                    <template v-slot:badge>
                        <span v-if="message > 0"> {{ message }}</span>
                    </template>
                    <v-btn router to="/message" flat icon transparent>
                        <v-icon color="white" large >mdi-email</v-icon>
                    </v-btn>
                </v-badge>

                

                <v-menu offset-y class="mr-3">
                    <template v-slot:activator="{ on }">
                        <v-btn transparent icon  v-on="on">
                            <v-icon>mdi-dots-vertical</v-icon>
                        </v-btn>
                    </template>
                    <v-list>
                        <v-list-item v-for="item in menuItem" :key="item.name" router :to="item.route">
                            <v-list-item-title>{{item.name}}</v-list-item-title>
                        </v-list-item>
                    </v-list>
                </v-menu>
            </v-app-bar>
            <div class="app_title_box white">
                
                <h2 class="app_title">Lists</h2>
                <v-text-field
                    class="app_search"
                    v-model="search"
                    append-icon="mdi-search"
                    label="Search"
                    single-line
                    hide-details
                    background-color="white"
                ></v-text-field>
            </div>
            <v-data-table
                v-model="selected"
                :headers="headers"
                :items="desserts"
                :search="search"
                :loading="loading"
                loading-text="در حال لود...لطفا منتظر باشید"
                item-key="name"
                show-select
                class="elevation-1"
            >
            <template v-slot:item.fat="{ item }">
                <v-chip :color="getColorFat(item.fat)" dark>{{ item.fat }}</v-chip>
            </template>
            
            </v-data-table>
        </div>
    </v-container>
</template>

<script>
export default {
    data(){
        return{

            message: 5,
            menuItem:[
                {name: "item1", route: '/1'},
                {name: "item2", route: '/2'},
                {name: "item3", route: '/3'},
                {name: "item4", route: '/4'},
                {name: "item5", route: '/5'},
            ],

            //
            search:'',
            selected:[],
            loading:false,
            headers: [
                {
                    text: 'دسر(100گرم سرو)',
                    align: 'left',
                    sortable: true,
                    value: 'name',
                },
                { text: 'Calories', value: 'calories' },
                { text: 'Fat (g)', value: 'fat' },
                { text: 'Carbs (g)', value: 'carbs' },
                { text: 'Protein (g)', value: 'protein' },
                { text: 'Iron (%)', value: 'iron' },
            ],
            desserts: [
                {
                    name: 'ماست یخی',
                    calories: 159,
                    fat: 6.0,
                    carbs: 24,
                    protein: 4.0,
                    iron: '1%',
                },
                {
                    name: 'بستنی نونی',
                    calories: 237,
                    fat: 9.0,
                    carbs: 37,
                    protein: 4.3,
                    iron: '1%',
                },
                {
                    name: 'شیرینی خامه ای',
                    calories: 262,
                    fat: 16.0,
                    carbs: 23,
                    protein: 6.0,
                    iron: '7%',
                },
                {
                    name: 'کاپ کیک',
                    calories: 305,
                    fat: 3.7,
                    carbs: 67,
                    protein: 4.3,
                    iron: '8%',
                },
                {
                    name: 'نون زنجبیلی',
                    calories: 356,
                    fat: 16.0,
                    carbs: 49,
                    protein: 3.9,
                    iron: '16%',
                },
                {
                    name: 'ژله',
                    calories: 375,
                    fat: 0.0,
                    carbs: 94,
                    protein: 0.0,
                    iron: '0%',
                },
                {
                    name: 'لالی پاپ',
                    calories: 392,
                    fat: 0.2,
                    carbs: 98,
                    protein: 0,
                    iron: '2%',
                },
                {
                    name: 'چیچی عسلی',
                    calories: 408,
                    fat: 3.2,
                    carbs: 87,
                    protein: 6.5,
                    iron: '45%',
                },
                {
                    name: 'دونات',
                    calories: 452,
                    fat: 25.0,
                    carbs: 51,
                    protein: 4.9,
                    iron: '22%',
                },
                {
                    name: 'کیت کت',
                    calories: 518,
                    fat: 26.0,
                    carbs: 65,
                    protein: 7,
                    iron: '6%',
                },
            ],
        }
    },
    methods:{
        getColorFat(fat){
            if(fat > 17) return 'red'
            else if(fat > 7) return 'orange'
            else return 'green'
        },
    }
}
</script>

<style scoped>
.app_title_box{
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    align-items: baseline;
    padding: 20px;
}
.app_title_box .app_title{
    flex: 1;
}
.app_title_box .app_search{
    flex: 1;
}
</style>
