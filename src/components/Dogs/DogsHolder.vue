<template>
  <div class="container">
    <DataFilter @input="onSearch"></DataFilter>
    <ListItem v-for="(dog) in getDogs"
      :key="dog._id"
      :guid="dog._id"
      :isSold="dog.isSold"
      :picture="dog.picture"
      :age="dog.age"
      :eyeColor="dog.eyeColor"
      :name="dog.name"
      :gender="dog.gender"
      :about="dog.about"
      :type="'dog'"
      :onDelete="onDeleted"
    >
    </ListItem>
  </div>
</template>

<script>
  import dogsService from '../../api/services/dogsService';
  import ListItem from '../shared/ListItem';
  import DataFilter from '../shared/DataFilter';

  export default {
    name: 'DogsHolder',
    components: {
      ListItem,
      DataFilter
    },
    data() {
      return {
        dogs: [],
        temp: [],
      };
    },
    computed: {
      getDogs() {
        return this.dogs;
      },
    },
    created() {
      this.getAllDogs();
    },
    methods: {
      getAllDogs() {
        // dogsService.getAllDogs().then((dogs) => {          
        //   this.dogs = dogs.data;
        //   this.temp = [...dogs.data];
        //   this.dogs.forEach(dog => {
        //     dogsService.getDogImage().then(res => {
        //       dog.picture = res.data.url;
        //     });
        //   });
        // });

        dogsService.getAllDogsGraphQl((dogs) => {          
          this.dogs = dogs;
          this.temp = [...dogs];
          this.dogs.forEach(dog => {
            dogsService.getDogImage().then(res => {
              dog.picture = res.data.url;
            });
          });
        });
      },
      onSearch(input) {
        const filtered = this.temp.filter((dogs) => {
        return dogs.name.toLocaleLowerCase().indexOf(input) !== -1
          || !input;
        });

        this.dogs = [...filtered];
      },
      onDeleted(id) {
        const index = this.cats.findIndex(cat => cat._id === id);
        this.cats.splice(index, 1);
        this.cats = cats;
        this.temp = [...cats];
      }
    },
  };
</script>
