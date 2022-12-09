<template>
  <div class="jumbotron vertical-center">
      <div class = "container">
          <div class = "row">
              <div class = "col-sm-12">
                  <h1>Recipes</h1>
                  <hr />
                  <!-- Allert Message -->
        <b-alert v-if="showMessage" variant="success" show>{{
          message
        }}</b-alert>
        <!-- b-alert v-if="error" variant="danger" show>{{ error }}</b-alert-->

        <button
          type="button"
          class="btn btn-success btn-sm"
          v-b-modal.recipe-modal
        >
          Create Recipe
        </button>
                  <br /> <br />
                  <table class = "table table-hover">
                      <thead>
                          <tr>
                              <th scope = "col"> Recipe Name </th>
                              <th scope = "col"> Recipe Ingredients </th>
                              <th scope = "col"> Recipe Instructions </th>
                              <th scope = "col"> Recipe Favorite </th>
                              <th scope = "col"> Recipe Rating </th>
                          </tr>
                      </thead>
                      <tbody>
                          <tr v-for="recipe in recipes" :key = "recipe.id">
                              <td> {{ recipe.name }} </td>
                              <td> {{ recipe.ingredients }} </td>
                              <td> {{ recipe.instructions }} </td>
                              <td>
                                  <b-icon 
                                  v-if="recipe.favorite == true" icon="heart-fill">
                                  </b-icon>
                                  <b-icon v-if="(recipe.favorite == false)" icon="heart">
                                  </b-icon>
                              </td>
                              
                               <td> 
                                  <div class="btn-group" role="group">
                                      <button
                                      type="button"
                                      class="btn btn-info btn-sm"
                                      v-b-modal.edit-recipe-modal
                                      @click="editRecipe(recipe)"
                                      >
                                      Edit
                                      </button>
                                      <button
                                      type="button"
                                      class="btn btn-danger btn-sm"
                                      @click="deleteRecipe(recipe)"
                                      >
                                      Delete
                                      </button>
                                  </div>
                              </td>
                          </tr>
                      </tbody>
                  </table>
                  <footer class = "text-center">
                      <p> &copy; 2022- 2023 </p>
                  </footer>
              
              </div>
          </div>
          <!-- Beginning of Modal for Create Recipe-->
              <div>   <b-modal 
              ref = "addRecipeModal"
              id="recipe-modal"
              title="Add Recipe"
              hide-backdrop
              hide-footer
              >
              <b-form @submit="onSubmit" class="w-100">
              <b-form-group
                  id="form-name-group"
              label="Recipe Name:"
              label-for="form-name-input"
              description="Enter the name of the recipe."
              >
              <b-form-input
              id="form-name-input"
              type="text"
              v-model="createRecipeForm.name"
              placeholder="Recipe Name"
              required
              >
              </b-form-input>
              </b-form-group>
              <b-form-group
              id="form-ingredients-group"
              label="Recipe Ingredients:"
              label-for="form-ingredients-input"
              description="Enter the ingredients of the recipe."
              >
              <b-form-input
              id="form-ingredients-input"
              type="text"
              v-model="createRecipeForm.ingredients"
              placeholder="Recipe Ingredients"
              required
              ></b-form-input>
              </b-form-group>
              <b-form-group
              id="form-instructions-group"
              label="Recipe Instructions:"
              label-for="form-instructions-input"
              description="Enter the instructions of the recipe."
              >
              <b-form-input
              id="form-instructions-input"
              type="text"
              v-model="createRecipeForm.instructions"
              placeholder="Recipe Instructions"
              required
              ></b-form-input>
              </b-form-group>
              <b-form-group
              id="form-favorite-group"    
              label="Recipe Favorite:"
              label-for="form-favorite-input"
              description="Let us know if you want to favorite this recipe"
              >
              <b-form-checkbox
              id="form-favorite-input"
              v-model="createRecipeForm.favorite"
              switch
              ></b-form-checkbox>
              </b-form-group>
              <b-form-group
              id="form-rating-group"
              label="Recipe Rating:"
              label-for="form-rating-input"
              description="Rate this recipe from 1-5"
              >
              <b-form-rating
              id="form-rating-input"
              type="integer"
              v-model="createRecipeForm.rating"
              ></b-form-rating>
              </b-form-group>
              <b-button type="submit" variant="primary">Submit</b-button>
              </b-form>
          </b-modal>

          <!-- End of Modal for Create Recipe-->

          <!-- Beginning of Modal for Edit Recipe-->
          <b-modal
          ref = "editRecipeModal"
          id="edit-recipe-modal"
          title="Edit Recipe"
          hide-backdrop
          hide-footer
          >
          <b-form @submit="onSubmitUpdate" class="w-100">
          <b-form-group
          id="form-name-group"
          label="Recipe Name:"
          label-for="form-name-input"
          description="Enter the name of the recipe."
          >
          <b-form-input
          id="form-name-input"
          type="text"
          v-model="editRecipeForm.name"
          placeholder="Recipe Name"
          
          >
          </b-form-input>
          </b-form-group>
          <b-form-group
          id="form-ingredients-group"
          label="Recipe Ingredients:"
          label-for="form-ingredients-input"
          description="Enter the ingredients of the recipe."
          >
          <b-form-input
          id="form-ingredients-input"
          type="text"
          v-model="editRecipeForm.ingredients"
          placeholder="Recipe Ingredients"
          >
          </b-form-input>
          </b-form-group>
          <b-form-group
          id="form-instructions-group"
          label="Recipe Instructions:"
          label-for="form-instructions-input"
          description="Enter the instructions of the recipe."
          >
          <b-form-input

          id="form-instructions-input"
          type="text"
          v-model="editRecipeForm.instructions"
          placeholder="Recipe Instructions"
          >
          </b-form-input>
          </b-form-group>

          <b-form-group

          id="form-favorite-group"
          label="Recipe Favorite:"
          label-for="form-favorite-input"
          description="Let us know if you want to favorite this recipe"
          >
          <b-form-checkbox
          id="form-favorite-input"
          v-model="editRecipeForm.favorite"
          switch
          ></b-form-checkbox>
          </b-form-group>
          <b-form-group
          id="form-rating-group"
          label="Recipe Rating:"
          label-for="form-rating-input"
          description="Rate this recipe from 1-5"
          >
          <b-form-rating
          id="form-rating-input"
          type="integer"
          v-model="editRecipeForm.rating"
          ></b-form-rating>
          </b-form-group>
  
          <b-button type="submit" variant="primary">Update</b-button>
          </b-form>
          </b-modal>
          <!-- End of Modal for Edit Recipe-->

      </div>
  
  </div>
</div>
 
  

</template>

<script>


import axios from 'axios'

export default
{
  name: 'AppRecipes',
  data()
  {
      return {
          recipes: [],
          createRecipeForm: {
              name: '',
              ingredients: '',
              instructions: '',
              favorite: false,
              rating: 0
          },

          editRecipeForm: {
              name: '',
              ingredients: '',
              instructions: '',
              favorite: false,
              rating: 0
          },

          showMessage: false,
          message: '',

      };
  },
  methods: {
      //GET Recipes
      RESTgetRecipes() {
      const path = `${process.env.VUE_APP_ROOT_URL}/recipes`;
      axios
          .get(path)
          .then((response) => {
              this.recipes = response.data.recipes;
          })
          .catch((error) => {
              console.log(error);
          });
      },

      //POST Recipes
      RESTcreateRecipe(payload){
          const path = `${process.env.VUE_APP_ROOT_URL}/recipes`;
          axios
              .post(path,payload)
              .then((response) => {
                  this.RESTgetRecipes();
                  this.message = "Account Created succesfully!";
                  // To actually show the message
                  this.showMessage = true;
                  // To hide the message after 3 seconds
                  setTimeout(() => {
                      this.showMessage = false;
                  }, 3000);
              })
              .catch((error) => {
                  console.log(error);
                  this.RESTgetRecipes()
              });
      },

      RESTupdateRecipe(payload, recipeId){
          const path = `${process.env.VUE_APP_ROOT_URL}/recipes/${recipeId}`;
          axios
              .put(path,payload)
              .then((response) => {
                  this.RESTgetRecipes();
              })
              .catch((error) => {
                  console.log(error);
                  this.RESTgetRecipes()
              });
      },


      RESTdeleteRecipe(id){
          const path = `${process.env.VUE_APP_ROOT_URL}/recipes/${id}`;
          axios
              .delete(path)
              .then((response) => {
                  this.RESTgetRecipes();
                  this.message = "Recipe Deleted Successfully"
                  this.showMessage = true;
                  set.TimeOut(() => {
                      this.showMessage = false;
                  }, 3000);

              })
              .catch((error) => {
                  console.log(error);
                  this.RESTgetRecipes()
              });
      },

      initForm(){
          this.createRecipeForm = {
              name: '',
              ingredients: '',
              instructions: '',
              favorite: false,
              rating: 0
          };
          this.updateRecipeForm = {
              name: '',
              ingredients: '',
              instructions: '',
              favorite: false,
              rating: 0
          };

      },
      onSubmit(evt) {
          evt.preventDefault(); 
          this.$refs.addRecipeModal.hide(); 
          const payload = {
              name: this.createRecipeForm.name,
              ingredients: this.createRecipeForm.ingredients,
              instructions: this.createRecipeForm.instructions,
              favorite: this.createRecipeForm.favorite,
              rating: this.createRecipeForm.rating
          };
          this.RESTcreateRecipe(payload);
          this.initForm();
      },
      onSubmitUpdate(evt) {
          evt.preventDefault(); 
          this.$refs.editRecipeModal.hide(); 
          const payload = {
              name: this.editRecipeForm.name,
              ingredients: this.editRecipeForm.ingredients,
              instructions: this.editRecipeForm.instructions,
              favorite: this.editRecipeForm.favorite,
              rating: this.editRecipeForm.rating
          };
          this.RESTupdateRecipe(payload);
          this.initForm();
      },

      deleteRecipe(id) {
          this.RESTdeleteRecipe(id);
      },

      editRecipe(recipe) {
          this.RESTeditRecipe(recipe);
      },

      

  },

   /***************************************************
 * LIFECYClE HOOKS
 ***************************************************/

  created() {
      this.RESTgetRecipes();
  },
};

</script>
