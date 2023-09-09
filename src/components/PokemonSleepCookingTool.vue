<template>
<div>

    <v-system-bar>
        <a :href="homepage" target="_blank" rel="noopener">Homepage</a>

        <v-btn size="small" variant="outlined" color="primary" @click="save">save</v-btn>
        <v-btn size="small" variant="outlined" color="primary" @click="load">load</v-btn>

        <span class="ms-2">v{{ version }}</span>
    </v-system-bar>

    <v-container>
        <v-row no-gutters align="end">
            <v-col v-for="(ingredient, name, index_ingredients) in ingredients" :key="index_ingredients" cols="1">
                <v-card>
                    <v-card-text>
                        <!-- <v-avatar color="surface-variant" rounded="0"> -->
                        <v-img :src="'image/Ingredient/' + ingredient.lowercase + '.png'" alt="ingredient.lowercase"></v-img>
                        <!-- </v-avatar> -->

                        <p class="ingredient-name">
                            {{ ingredient.name }}
                        </p>

                    </v-card-text>

                    <v-text-field v-model="ingredient.value" type="number" :tabindex="index_ingredients + 10">
                        <!-- <template #details style="display: none"></template> -->
                    </v-text-field>

                    <v-card-actions>
                        <v-btn size="small" color="surface-variant" variant="text" icon="mdi-plus" @click="ingredient.value++"></v-btn>
                        <v-btn size="small" color="surface-variant" variant="text" icon="mdi-minus" @click="ingredient.value--"></v-btn>
                    </v-card-actions>
                </v-card>
            </v-col>
        </v-row>
    </v-container>

    <v-divider></v-divider>

    <v-container>
        <v-row no-gutters>
            <v-col v-for="(recipeCategory, name, recipes_index) in recipes" :key="recipes_index" cols="12" md="6" lg="4">
                <v-card>
                    <v-list lines="false">

                        <v-list-item v-for="(recipe, name, index_recipeCategory) in recipeCategory" :key="index_recipeCategory" :class="recipe.able ? 'able' : 'unable'" v-show="true || recipe.able || showAll" :base-color="getRecipeColor(recipe.able)"
                            color="#d3d3d3" :active="recipe.collected">

                            <v-list-item-title>

                                <v-avatar size="3.5em">
                                    <v-img class="recipe-image" :src="'image/' + recipe.category + '/' + recipe.lowercase + '.png'" alt="recipe.name.lowercase"></v-img>
                                </v-avatar>

                                <span>
                                    {{ recipe.name }} &lt;{{ recipe.size }}&gt;
                                </span>

                            </v-list-item-title>

                            <v-list-item-subtitle>
                                <div v-for="(recipe_value, recipe_ingredient, index_ingredients) in recipe.ingredients" :key="index_ingredients">

                                    <span :style="'color: ' + getChipColor(recipe_value, recipe_ingredient)">[{{this.ingredients[recipe_ingredient].value}}]</span>

                                    <v-avatar start>
                                        <v-img class="recipe-ingredient-image" :src="'image/Ingredient/' + this.ingredients[recipe_ingredient].lowercase + '.png'" alt="ingredient.lowercase"></v-img>
                                    </v-avatar>

                                    <v-chip :color="getChipColor(recipe_value, recipe_ingredient)">
                                        {{recipe_ingredient}} * {{recipe_value}}
                                    </v-chip>

                                </div>
                            </v-list-item-subtitle>
                            <div style="display: inline-flex; width: 80%;">
                                <v-checkbox-btn color="success" tabindex="-1" v-model="recipe.collected" label="collected" hide-details></v-checkbox-btn>
                            </div>

                            <v-divider></v-divider>

                        </v-list-item>



                    </v-list>
                </v-card>
            </v-col>
        </v-row>
    </v-container>

    <v-snackbar v-model="snackbar" :color="snackbar_payload.color" timeout="2000">
        {{ snackbar_payload.text }}

        <template v-slot:actions>
            <v-btn variant="text" @click="snackbar = false">
                Close
            </v-btn>
        </template>

    </v-snackbar>

</div>
</template>

<script>
import { GetRecipes } from '../common/recipes.js';
import CONFIG from '@/../package.json';
const VERSION = CONFIG.version;
const HOMEPAGE = CONFIG.homepage;

export default {
    props: [
    ],
    data() {
        return {
            homepage: HOMEPAGE,
            version: VERSION,

            autoCalc: true,
            //allRecipes: {},
            recipes: {},
            ingredients: {
                'Large Leek':
                {
                    name: 'Large Leek',
                    lowercase: 'largeleek',
                    value: 0,
                },
                'Tasty Mushroom':
                {
                    name: 'Tasty Mushroom',
                    lowercase: 'tastymushroom',
                    value: 0,
                },
                'Fancy Egg':
                {
                    name: 'Fancy Egg',
                    lowercase: 'fancyegg',
                    value: 0,
                },
                'Soft Potato':
                {
                    name: 'Soft Potato',
                    lowercase: 'softpotato',
                    value: 0,
                },
                'Fancy Apple':
                {
                    name: 'Fancy Apple',
                    lowercase: 'fancyapple',
                    value: 0,
                },
                'Fiery Herb':
                {
                    name: 'Fiery Herb',
                    lowercase: 'fieryherb',
                    value: 0,
                },
                'Bean Sausage':
                {
                    name: 'Bean Sausage',
                    lowercase: 'beansausage',
                    value: 0,
                },
                'Moomoo Milk':
                {
                    name: 'Moomoo Milk',
                    lowercase: 'moomoomilk',
                    value: 0,
                },
                'Honey':
                {
                    name: 'Honey',
                    lowercase: 'honey',
                    value: 0,
                },
                'Pure Oil':
                {
                    name: 'Pure Oil',
                    lowercase: 'pureoil',
                    value: 0,
                },
                'Warming Ginger':
                {
                    name: 'Warming Ginger',
                    lowercase: 'warmingginger',
                    value: 0,
                },
                'Snoozy Tomato':
                {
                    name: 'Snoozy Tomato',
                    lowercase: 'snoozytomato',
                    value: 0,
                },
                'Soothing Cacao':
                {
                    name: 'Soothing Cacao',
                    lowercase: 'soothingcacao',
                    value: 0,
                },
                'Slowpoke Tail':
                {
                    name: 'Slowpoke Tail',
                    lowercase: 'slowpoketail',
                    value: 0,
                },
                'Greengrass Soybeans':
                {
                    name: 'Greengrass Soybeans',
                    lowercase: 'greengrasssoybeans',
                    value: 0,
                },

            },
            pot_size: 15,
            sunday_mode: false,
            showAll: true,

            snackbar: false,
            snackbar_payload: {
                text: '',
                color: '',
            },
        };
    },
    methods: {
        whoami() {
            // console.log(this.ingredients);
        },
        save() {
            let ingredients_payload = JSON.stringify(this.ingredients);
            let recipes_payload = JSON.stringify(this.recipes);

            localStorage.setItem('pokemonsleeprecipetool_ingredients_payload', ingredients_payload);
            localStorage.setItem('pokemonsleeprecipetool_pot_size_payload', this.pot_size);
            localStorage.setItem('pokemonsleeprecipetool_recipes_payload', recipes_payload);

            this.snackbar_payload.text = 'data saved';
            this.snackbar_payload.color = 'info';
            this.snackbar = true;
        },
        load() {
            this.autoCalc = false;

            let str1 = localStorage.getItem('pokemonsleeprecipetool_ingredients_payload');
            let str2 = localStorage.getItem('pokemonsleeprecipetool_pot_size_payload');
            let str3 = localStorage.getItem('pokemonsleeprecipetool_recipes_payload');

            if (!str1 || !str2 || !str3) {
                //no saved data
                console.log('no saved data or saved data corrupted');

                this.snackbar_payload.text = 'no saved data or saved data corrupted';
                this.snackbar_payload.color = 'error';
                this.snackbar = true;

                return false;
            }

            let ingredients_payload = JSON.parse(str1);
            for (const savedIngredient in ingredients_payload) {
                this.ingredients[savedIngredient].value = ingredients_payload[savedIngredient].value;
            }

            let pot_size_payload = str2;
            this.pot_size = pot_size_payload;

            let recipes_payload = JSON.parse(str3);
            for (const recipeCategory in recipes_payload) {
                for (const recipe in recipes_payload[recipeCategory]) {
                    this.recipes[recipeCategory][recipe].collected = recipes_payload[recipeCategory][recipe].collected;
                }
            }


            this.autoCalc = true;

            this.snackbar_payload.text = 'saved data loaded';
            this.snackbar_payload.color = 'success';
            this.snackbar = true;


            //this.calc();
            //this.ingredients = {};
            //this.ingredients = payload;
        },
        /*
        getRecipeClass(able) {
            console.log('getRecipeClass');
            console.log(able);
            if (able) {
                return 'able';
            } else {
                if (this.showAll) {
                    return 'unable';
                } else {
                    return 'hide';
                }
            }
        },
        */
        calc() {
            //this.recipes[recipeCategory][recipe].able
            //1 able
            //-1 not enough ingredients, unable.
            //-2 not enough pot_size
            //-2 not enough pot_size but possible when sunday
            for (const recipeCategory in this.recipes) {
                //console.log(recipeCategory);
                for (const recipe in this.recipes[recipeCategory]) {

                    //console.log(this.recipes[recipeCategory]);
                    //console.log(recipe);

                    this.recipes[recipeCategory][recipe].able = 1;

                    for (const ingredient in this.recipes[recipeCategory][recipe].ingredients) {
                        //console.log(ingredient);
                        //console.log(this.recipes[recipe].ingredients[ingredient]);
                        //console.log(this.ingredients[ingredient].value);

                        if (this.ingredients[ingredient].value < this.recipes[recipeCategory][recipe].ingredients[ingredient]) {
                            //not enough ingredients, unable.
                            this.recipes[recipeCategory][recipe].able = -1;
                            //console.log('unable to make: ' + recipe);
                            break;
                        } else {
                            //console.log('continue');
                        }
                    }

                    //console.log(this.recipes[recipe].able);
                }
            }


        },
        getRecipeColor(able) {
            //console.log('getRecipeColor');
            //console.log(value);
            //console.log(able);
            //console.log(this.ingredients[ingredient].value);
            if (false) {
                //learned
                return 'grey';
            }

            if (able === 1) {

                return 'green';

            } else {
                if (false) {


                } else {
                    return 'red';
                }

            }

            //learned
            //able
            //unable

            //not enough ingredients
            //not enough pot
            //not enough pot but is possible when sunday


        },
        getChipColor(value, ingredient) {
            //console.log('getChipColor');
            //console.log(value);
            //console.log(ingredient);
            //console.log(this.ingredients[ingredient].value);
            if (this.ingredients[ingredient].value >= value) {
                return '#006400';
            } else {
                return 'darkred';
            }
        },
        loadRecipes() {
            let recipes = GetRecipes();
            for (const recipeCategory in recipes) {
                for (const recipe in recipes[recipeCategory]) {

                    //console.log(recipes[recipeCategory][recipe]);

                    let size = 0;

                    for (const ingredient in recipes[recipeCategory][recipe].ingredients) {
                        size = size + recipes[recipeCategory][recipe].ingredients[ingredient];
                    }
                    recipes[recipeCategory][recipe].size = size;
                    //console.log(recipes[recipeCategory][recipe]);
                }
            }
            //console.log(recipes);
            this.recipes = recipes;
        },
    },
    beforeMount() {

    },
    mounted() {
        this.loadRecipes();
    },
    watch: {
        ingredients: {
            handler(new_val, old_val) {
                /*
                console.log('old_val:');
                console.log(old_val);
                console.log('new_val:');
                console.log(new_val);
                //console.log(this.ingredients);
                */
                if (this.autoCalc) {
                    this.calc();
                }
            },
            deep: true,
        },
        recipes: {
            handler(new_val, old_val) {
                /*
                console.log('old_val:');
                console.log(old_val);
                console.log('new_val:');
                console.log(new_val);
                //console.log(this.ingredients);
                */
            },
            deep: true,
        },
    },
};
</script>

<style>
.able {}

.unable {
    /* background-color: grey; */
}

.hide {
    display: none;
}

.ingredient-name {
    height: 2em;
    display: inline-flex;
}

.test {

}

.v-text-field .v-input__details {
    display: none;
}

.recipe-title {
    /* font-size: 2em; */
}

.recipe-ingredient-image {
    /* height: 2em; */
}
</style>
