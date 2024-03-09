<template>
<div>

    <v-system-bar>
        <a :href="homepage" tabindex="1" target="_blank" rel="noopener">Homepage</a>

        <v-btn size="small" variant="outlined" color="primary" @click="save" tabindex="2">save</v-btn>
        <v-btn size="small" variant="outlined" color="primary" @click="load" tabindex="3">load</v-btn>

        <v-btn size="small" variant="outlined" color="primary" @click="checkWanted" tabindex="4">wanted</v-btn>

        <span class="ms-2">v{{ version }}</span>
    </v-system-bar>

    <!-- Main ingredients Part -->
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

                    <v-text-field v-model="ingredient.value" type="number" :tabindex="index_ingredients + 100">
                    </v-text-field>

                    <v-card-actions>
                        <v-btn tabindex="-1" size="small" color="surface-variant" variant="text" icon="mdi-plus" @click="ingredient.value++"></v-btn>
                        <v-btn tabindex="-1" size="small" color="surface-variant" variant="text" icon="mdi-minus" @click="ingredient.value--"></v-btn>
                    </v-card-actions>
                </v-card>
            </v-col>
        </v-row>
    </v-container>

    <v-divider></v-divider>

    <!-- Main Recipe Part -->
    <v-container>
        <v-row no-gutters>
            <v-col v-for="(recipeCategory, name, recipes_index) in recipes" :key="recipes_index" cols="12" md="6" lg="4">
                <v-card>
                    <v-list lines="false">

                        <v-list-item v-for="(recipe, name, index_recipeCategory) in recipeCategory" :key="index_recipeCategory" :active="recipe.collected" :base-color="recipe.collected ? 'grey' : ''">

                            <v-list-item-title>

                                <Dish :recipe="recipe" :able="recipe.able" :collected="recipe.collected"/>

                            </v-list-item-title>

                            <v-list-item-subtitle>
                                <div v-for="(recipe_value, recipe_ingredient, index_ingredients) in recipe.ingredients" :key="index_ingredients">

                                    <Ingredient :ingredient_recipe_value="recipe_value" :ingredient_name="recipe_ingredient" :ingredients="ingredients" :collected="recipe.collected"/>

                                </div>
                            </v-list-item-subtitle>
                            <div style="display: inline-flex; width: 80%;">
                                <v-checkbox-btn color="orange" tabindex="index_recipeCategory * 2 + 1000" v-model="wanted" :value="recipe.name" label="wanted" hide-details></v-checkbox-btn>
                                <v-checkbox-btn color="success" tabindex="index_recipeCategory * 2 + 1001" v-model="recipe.collected" label="collected" hide-details></v-checkbox-btn>
                            </div>

                            <v-divider></v-divider>

                        </v-list-item>



                    </v-list>
                </v-card>
            </v-col>
        </v-row>
    </v-container>

    <!-- Notice Part -->
    <v-snackbar v-model="snackbar" :color="snackbar_payload.color" timeout="2000">
        {{ snackbar_payload.text }}

        <template v-slot:actions>
            <v-btn variant="text" @click="snackbar = false">
                Close
            </v-btn>
        </template>

    </v-snackbar>

    <!-- Wanted Modal Part -->
    <v-dialog v-model="dialog" width="auto">
        <v-card>
            <v-card-title>
                Wanted
            </v-card-title>

            <v-card-text v-if="wanted.length">
                You want: <br>

                <v-list-item v-for="(recipe, name) in requiredRecipes">
                    <Dish :recipe="recipe" />

                </v-list-item>

                You will need these ingredients <br>

                <div v-for="(required_ingredients_obj, required_ingredient_name, required_ingredient_index) in requiredIngredients" :key="required_ingredient_index">

                    <Ingredient :ingredient_recipe_value="required_ingredients_obj.value" :ingredient_name="required_ingredient_name" :ingredients="ingredients" />

                </div>

            </v-card-text>

            <v-card-text v-else="wanted">
                Check the recipes you want to collect.
            </v-card-text>

            <v-card-actions>
                <v-btn color="primary" block @click="dialog = false">Close Dialog</v-btn>
            </v-card-actions>
        </v-card>
    </v-dialog>

</div>
</template>

<script>
import { GetIngredients, GetRecipes } from '../common/gamedatacollection.js';
import CONFIG from '@/../package.json';

import Dish from '../subcomponents/Dish.vue';
import Ingredient from '../subcomponents/Ingredient.vue';

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
            ingredients: {},
            pot_size: 15,
            sunday_mode: false,
            showAll: true,

            snackbar: false,
            snackbar_payload: {
                text: '',
                color: '',
            },

            dialog: false,
            wanted: [],
            requiredRecipes: {},
            requiredIngredients: {},
        };
    },
    components: {
        Dish,
        Ingredient,
    },
    methods: {
        save() {
            let ingredients_payload = JSON.stringify(this.ingredients);
            let recipes_payload = JSON.stringify(this.recipes);
            let wanted_payload = JSON.stringify(this.wanted);

            localStorage.setItem('pokemonsleeprecipetool_ingredients_payload', ingredients_payload);
            localStorage.setItem('pokemonsleeprecipetool_pot_size_payload', this.pot_size);
            localStorage.setItem('pokemonsleeprecipetool_recipes_payload', recipes_payload);
            localStorage.setItem('pokemonsleeprecipetool_wanted_payload', wanted_payload);

            this.snackbar_payload.text = 'data saved';
            this.snackbar_payload.color = 'info';
            this.snackbar = true;
        },
        load() {
            this.autoCalc = false;

            let str1 = localStorage.getItem('pokemonsleeprecipetool_ingredients_payload');
            let str2 = localStorage.getItem('pokemonsleeprecipetool_pot_size_payload');
            let str3 = localStorage.getItem('pokemonsleeprecipetool_recipes_payload');
            let str4 = localStorage.getItem('pokemonsleeprecipetool_wanted_payload');

            if (str1) {
                let ingredients_payload = JSON.parse(str1);
                for (const savedIngredient in ingredients_payload) {
                    this.ingredients[savedIngredient].value = ingredients_payload[savedIngredient].value;
                }
            }

            if (str2) {
                let pot_size_payload = str2;
                this.pot_size = pot_size_payload;
            }

            if (str3) {
                let recipes_payload = JSON.parse(str3);
                for (const recipeCategory in recipes_payload) {
                    for (const recipe in recipes_payload[recipeCategory]) {
                        this.recipes[recipeCategory][recipe].collected = recipes_payload[recipeCategory][recipe].collected;
                    }
                }
            }

            if (str4) {
                let wanted_payload = JSON.parse(str4);
                this.wanted = wanted_payload;
            }

            this.autoCalc = true;

            this.snackbar_payload.text = 'saved data loaded';
            this.snackbar_payload.color = 'success';
            this.snackbar = true;

        },
        checkWanted() {
            this.dialog = true;

            let recipes = GetRecipes();

            const plainRecipe = {
                ...recipes.Curry,
                ...recipes.Salad,
                ...recipes.Dessert,
            };

            let requiredIngredients = GetIngredients();
            let requiredRecipes = {};

            this.wanted.forEach((wantedRecipeName, i) => {
                if (plainRecipe.hasOwnProperty(wantedRecipeName)) {

                    let category = plainRecipe[wantedRecipeName].category;
                    requiredRecipes[wantedRecipeName] = this.recipes[category][wantedRecipeName];

                    for (const wantedIngredient in plainRecipe[wantedRecipeName].ingredients) {
                        requiredIngredients[wantedIngredient].value = requiredIngredients[wantedIngredient].value + plainRecipe[wantedRecipeName].ingredients[wantedIngredient];
                    }
                } else {
                    console(wantedRecipeName + ' Not Found!');
                }
            });

            let requiredIngredientsTrim = {};

            for (const wantedIngredient in requiredIngredients) {
                if (requiredIngredients[wantedIngredient].value > 0) {
                    requiredIngredientsTrim[wantedIngredient] = requiredIngredients[wantedIngredient];
                }
            }

            this.requiredRecipes = requiredRecipes;
            this.requiredIngredients = requiredIngredientsTrim;
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
                for (const recipe in this.recipes[recipeCategory]) {

                    this.recipes[recipeCategory][recipe].able = 1;

                    for (const ingredient in this.recipes[recipeCategory][recipe].ingredients) {
                        if (this.ingredients[ingredient].value < this.recipes[recipeCategory][recipe].ingredients[ingredient]) {
                            //not enough ingredients, unable.
                            this.recipes[recipeCategory][recipe].able = -1;
                            break;
                        }
                    }
                }
            }
        },
        loadIngredients() {
            let ingredients = GetIngredients();
            this.ingredients = ingredients;
        },
        loadRecipes() {
            let recipes = GetRecipes();
            for (const recipeCategory in recipes) {
                for (const recipe in recipes[recipeCategory]) {

                    let size = 0;

                    for (const ingredient in recipes[recipeCategory][recipe].ingredients) {
                        size = size + recipes[recipeCategory][recipe].ingredients[ingredient];
                    }
                    recipes[recipeCategory][recipe].size = size;
                }
            }

            this.recipes = recipes;
        },
    },
    beforeMount() {
    },
    mounted() {
        this.loadIngredients();
        this.loadRecipes();
    },
    watch: {
        ingredients: {
            handler(new_val, old_val) {
                if (this.autoCalc) {
                    this.calc();
                }
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
