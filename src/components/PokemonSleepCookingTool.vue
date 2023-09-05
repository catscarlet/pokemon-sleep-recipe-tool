<template>
<div>
    <v-container>
        <v-row no-gutters align="end">
            <v-col v-for="(ingredient, name, index) in ingredients" :key="index" cols="1">
                <v-card>
                    <v-card-text>
                        <!-- <v-avatar color="surface-variant" rounded="0"> -->
                        <v-img :src="'image/Ingredient/' + ingredient.lowercase + '.png'" alt="ingredient.lowercase"></v-img>
                        <!-- </v-avatar> -->

                        <p class="ingredient-name">
                            <!-- {{ index + ':' + ingredient.name }}+ -->
                            {{ ingredient.name }}
                        </p>

                    </v-card-text>

                    <v-text-field v-model="ingredient.value" type="number" :tabindex="index + 10">
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
        <v-row>
            <v-col cols="3">

            </v-col>
            <!--
            <v-col cols="3">
                <v-text-field label="Pot Size" v-model="pot_size"></v-text-field>
            </v-col>
        -->

            <v-col cols="3">

                <v-btn @click="save">save</v-btn>
                <v-btn @click="load">load</v-btn>
            </v-col>
        </v-row>

        <!-- <v-btn size="small" @click="calc"> calc</v-btn> -->



        <!-- <v-switch label="Show All" v-model="showAll" inset color="primary"></v-switch> -->
    </v-container>

    <v-divider></v-divider>

    <v-container>
        <v-row no-gutters>
            <v-col v-for="(recipeCategory, name, index) in recipes" :key="index" cols="12" md="6" lg="4">
                <v-card>
                    <v-list lines="false">



                        <v-list-item v-for="(recipe, name, index) in recipeCategory" :key="index" :class="recipe.able ? 'able' : 'unable'" v-show="true || recipe.able || showAll" :base-color="getRecipeColor(recipe.able)">

                            <v-list-item-title>

                                <v-avatar size="3.5em">
                                    <v-img class="recipe-image" :src="'image/' + recipe.category + '/' + recipe.lowercase + '.png'" alt="recipe.name.lowercase"></v-img>
                                </v-avatar>

                                <span>
                                    {{ recipe.name }} &lt;{{ recipe.size }}&gt;
                                </span>

                            </v-list-item-title>

                            <v-list-item-subtitle>
                                <div v-for="(recipe_value, recipe_ingredient, index2) in recipe.ingredients" :key="index2">

                                    <span :style="'color: ' + getChipColor(recipe_value, recipe_ingredient)">[{{this.ingredients[recipe_ingredient].value}}]</span>

                                    <v-avatar start>
                                        <v-img class="recipe-ingredient-image" :src="'image/Ingredient/' + this.ingredients[recipe_ingredient].lowercase + '.png'" alt="ingredient.lowercase"></v-img>
                                    </v-avatar>

                                    <v-chip :color="getChipColor(recipe_value, recipe_ingredient)">
                                        {{recipe_ingredient}} * {{recipe_value}}
                                    </v-chip>

                                </div>
                            </v-list-item-subtitle>
                            <!--
                            <div style="display: inline-flex; width: 80%;">
                                <v-checkbox-btn color="yellow" value="red" label="wanted" hide-details></v-checkbox-btn>
                                <v-checkbox-btn color="green" value="red" label="done" hide-details></v-checkbox-btn>
                            </div> -->

                            <v-divider></v-divider>

                        </v-list-item>



                    </v-list>
                </v-card>
            </v-col>
        </v-row>
    </v-container>
</div>
</template>

<script>
import { GetRecipes } from '../common/recipes.js';
export default {
    props: [
    ],
    data() {
        return {
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
        };
    },
    methods: {
        whoami() {
            // console.log(this.ingredients);
        },
        save() {
            let payload = JSON.stringify(this.ingredients);
            //console.log(payload);

            localStorage.setItem('pokemonsleeprecipetool_ingredients_payload', payload);
            localStorage.setItem('pokemonsleeprecipetool_pot_size_payload', this.pot_size);
        },
        load() {
            this.autoCalc = false;
            let str = localStorage.getItem('pokemonsleeprecipetool_ingredients_payload');
            let payload = JSON.parse(str);
            //console.log(payload);

            for (const savedIngredient in payload) {
                this.ingredients[savedIngredient].value = payload[savedIngredient].value;
            }

            let pot_size_payload = localStorage.getItem('pokemonsleeprecipetool_pot_size_payload');

            //console.log(pot_size_payload);

            if (pot_size_payload) {
                this.pot_size = pot_size_payload;
            }

            this.autoCalc = true;
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
        showAll: {
            handler(new_val, old_val) {
                /*
                console.log('old_val:');
                console.log(old_val);
                console.log('new_val:');
                console.log(new_val);
                //console.log(this.ingredients);
                */
            },
            deep: false,
        },
    },
};
</script>

<style>
.able {}

.unable {
    background-color: gray;
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