<template>
	<v-container grid-list-md fluid>
		<v-layout wrap>
			<v-flex xs12 sm4 md3 v-for="pet in dogs" :key="pet.breed">
				
        <appDogVcard :dog="pet" @addToFavorites="addToFavorites"></appDogVcard>

			</v-flex>
		</v-layout>
	</v-container>
</template>

<script>
  import { Dogs } from "../data/dogs";
  import Dog from '../components/Dog.vue';
  import axios from 'axios';
  import { mapActions } from 'vuex';

  axios.defaults.baseURL = 'https://dog.ceo/api';

  export default {
    components: {
		appDogVcard: Dog,
	},
    data() {
      return {
        dogs: Dogs
      };
    },
    methods: {
  ...mapActions(["addToFavorites"])
},
created() {
  this.dogs.forEach(dog => {
     dog.img = "";
  });
  const linksArray = this.dogs.map(
        dog => "/breed/" + dog.breed + "/images/random"
      );
  axios.all(linksArray.map(link => axios.get(link)))
   .then(
     axios.spread((...res) => {
       this.dogs.forEach((dog, index) => {
         dog.img = res[index].data.message;
       });
     })
   );
  }
   };
</script>

<style scoped>
	p {
		margin: 0;
	}
</style>