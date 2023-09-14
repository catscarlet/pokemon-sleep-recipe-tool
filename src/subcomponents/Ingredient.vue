<template>
<div>
    <span :style="style">[ {{ ingredients[ingredient_name].value }} ]</span>

    <v-avatar start>
        <v-img :src="'image/Ingredient/' + ingredients[ingredient_name].lowercase + '.png'" alt="ingredient[ingredient_name].lowercase"></v-img>
    </v-avatar>

    <v-chip :color="color">
        {{ ingredient_name }} * {{ ingredient_recipe_value }}
    </v-chip>

</div>
</template>

<script>
export default {
    props: [
        'ingredient_recipe_value',
        'ingredient_name',
        'ingredients',
        'collected',
    ],
    data() {
        return {
            style: {},
            color: '',
        };
    },
    methods: {
        whoami() {
        },
        getChipColor() {

            if (this.ingredients[this.ingredient_name].value >= this.ingredient_recipe_value) {
                this.color = '#006400';
            } else {
                if (this.collected) {
                    this.color = null;
                } else {
                    this.color = '#8b0000';
                }
            }
        },
    },
    beforeMount() {
    },
    mounted() {
        this.getChipColor();
    },
    watch: {
        ingredients: {
            handler(new_val, old_val) {
                this.getChipColor();
            },
            deep: true,
        },
        collected: {
            handler(new_val, old_val) {
                this.getChipColor();
            },
            deep: true,
        },
        color: {
            handler(new_val, old_val) {
                this.style.color = this.color;
            },
        },
    },
};
</script>

<style>
</style>
