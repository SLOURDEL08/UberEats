<template>
  <div class="home">
    <div class="header">
      <img src="https://d3i4yxtzktqr9n.cloudfront.net/web-eats-v2/ee037401cb5d31b23cf780808ee4ec1f.svg">
      <div class="wrapper--input">
        <input v-model="user_search_restaurant" type="text" placeholder="De quoi avez vous envie ?">
        <div class="search">
          <div v-for="(restaurant, i) in search_restaurant" :key="i"   class="container--restaurant--search">
            <div class="wrapper--img">
              <img :src="restaurant.image"/>
            </div>
            <p>{{ restaurant.name }}</p>
          </div>
        </div>
      </div>
    </div>
    <div class="banner"></div>
    <RestaurantRow v-for="(data, i) in data_restaurant" :key="i" :three_restaurant="data"/>
  </div>
</template>

<script>
// IMPORT
import BDD from '../BDD'
import { onMounted, ref, watch } from 'vue'
// COMPONENTS
import RestaurantRow from '../components/RestaurantRow.vue'; // Assurez-vous que le chemin est correct

export default {
  name: 'HomePage',
  components: {
    RestaurantRow,
  },
  setup() {
    class Restaurant {
      constructor(name, note, image, drive_time) {
        this.name = name
        this.note = note
        this.image = image
        this.drive_time = drive_time
      }
    }

   let data_restaurant = ref([]);
   let all_restaurant = [];

   const makeDataRestaurant = () => {
      let three_restaurant = [];

      for (const restaurant of BDD) {
        const new_restaurant = new Restaurant(restaurant.name, restaurant.note, restaurant.image, restaurant.drive_time)
        // make all restaurant array
        all_restaurant.push(new_restaurant);

      if (three_restaurant.length === 2) {
                three_restaurant.push(new_restaurant);
                data_restaurant.value.push(three_restaurant);
                three_restaurant = [];
      } else {
        three_restaurant.push(new_restaurant);
      }
      }
   }

   //user search
   let user_search_restaurant = ref('');
   let search_restaurant = ref([]);

   watch(user_search_restaurant, new_value => {
    let regex = RegExp(new_value.toLowerCase());

    let new_search_restaurant = all_restaurant.filter( restaurant => regex.test(restaurant.name.toLowerCase()));

    new_value == 0 ? search_restaurant.value = [] : search_restaurant.value = new_search_restaurant;



   })


   onMounted(makeDataRestaurant);
   //  return
   return {
    data_restaurant,
    user_search_restaurant,
    search_restaurant,
   }
  },
};
</script>

<style lang="scss">
.home{
  .header{
    width:100%;
    height: 120px ;
    display: flex;
    justify-content: space-between;
    align-items: center;
  

  img{
    width:200px;
  }
.wrapper--input{
  position: relative;
  input{
    background-color: #f6f6f6;
    height: 60px;
    width: 400px;
    border:none;
    outline: none;
    padding-left:20px;
  }
  .search{
    position: absolute;
    top:100%;
    width:100%;
    background-color: #fff;

    .container--restaurant--search{
      display: flex;
      align-items: center;
      padding:10px;

      &:hover{
        background: #f6f6f6;
      }

      .wrapper--img{
        height: 60px;
        width: 60px;
        margin-right: 25px;
        border-radius: 50%;
        overflow: hidden;

        img{
          height: 100%;
          width: auto;
        }
      }

    }
  }
}
  }

  .banner{
    height: 200px;
    width: 100%;
    background-image: url('https://www.uber-assets.com/image/upload/f_auto,q_auto:eco,c_fill,w_1116,h_744/v1622579402/assets/6d/caa1da-ef21-426e-b3be-170af0054fa9/original/WelcomeToUberEats.jpg');
    background-position: center center;
    background-size: cover;
    
  }
}
</style>
