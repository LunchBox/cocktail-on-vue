<script setup>
import { ref, reactive } from "vue";

class Recipe {
  constructor(attrs) {
    this.name = null;
    this.items = [];
    this.preparation = null;
    Object.assign(this, attrs);
  }
}

const negroni = {
  name: "Negroni",
  items: [
    { name: "gin", parts: "3", unit: "cl" },
    { name: "sweet vermouth", parts: "3", unit: "cl" },
    { name: "campari", parts: "3", unit: "cl" },
  ],
  preparation: "Stir into glass over ice, garnish and serve.",
}

const ginfizz = {
  name: "Gin Fizz",
  items: [
    { name: "gin", parts: "4.5", unit: "cl" },
    { name: "lemon juice", parts: "3", unit: "cl" },
    { name: "simple syrup", parts: "1", unit: "cl" },
    { name: "soda water", parts: "-", unit: "cl" },
  ],
  preparation: "Shake all ingredients with ice cubes, except soda water. Pour into glass. Top with soda water."

}

console.log(new Recipe(negroni));

const recipes = ref([new Recipe(negroni), new Recipe(ginfizz)]);

const showForm = ref(false);

const recipeModel = ref(new Recipe());

const part_units = ["ml", "cl", "parts", "dash"]

function onSubmit(){
  recipes.value.push(recipeModel.value);
  recipeModel.value = new Recipe();
}
</script>

<template>
  <main>
    <a href="" @click.prevent="showForm">Add Recipe</a>
    <form @submit.prevent="onSubmit">
      <div>
        <label for="name">Name</label>
        <input id="name" type="text" v-model="recipeModel.name" required />
      </div>

      <div>
        <label for="items">Items</label>
  
        <div v-for="(item, idx) in recipeModel.items" :key="idx">
          <input id="item-name" type="text" v-model="item.name" required />
          <input id="item-parts" type="number" v-model="item.parts" style="width: 4em;" required />
          <input id="item-part_unit" type="string" v-model="item.unit" list="part_units" style="width: auto;"/>
        </div>

        <a href="" @click.prevent="recipeModel.items.push({ name: null, parts: null })">Add Item</a>

        <datalist id="part_units">
          <option v-for="p in part_units" :key="p" :value="p" />
        </datalist>
      </div>

      <div>
        <label for="preparation">Preparation</label>
        <textarea id="preparation" v-model="recipeModel.preparation"></textarea>
      </div>
      <div>
        <button>Save</button>
      </div>
    </form>
    <details v-for="recipe in recipes" :key="recipe.name">
      <summary>
        {{ recipe.name }}
      </summary>
      <div>
        <ul>
          <li v-for="item in recipe.items" :key="item.name">
            {{ item.name }}
            x {{ item.parts }}{{ item.unit }}
          </li>
        </ul>
        <p>{{ recipe.preparation }}</p>
      </div>
    </details>
  </main>
</template>

<style>
  * { line-height: 1.5; }
  form {
    margin: 1em 0;
  }

  label {
    display: block;
  }

  input {
  }

  textarea {
    width: 320px;
    height: 6em;
  }


</style>
