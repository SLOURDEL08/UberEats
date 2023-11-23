<template>
  <div class="restaurantpage--section">
      <div class="restaurant--infos--section">
        <RouterLink to="/">
          <div id="return">Retour à la home</div>
        </RouterLink>
        <img class="dyn--restaurant--img" :src="restaurant.image" alt=""/>
        <p>{{ restaurant.name }}</p>
        <p>Note du restaurant : {{ restaurant.note }}</p>
        <p>Temps de livraison : {{ restaurant.drive_time }}</p>
      </div>
      <div class="restaurant--menu--section">
        <div v-for="category in restaurant.menu" :key="category.category" >
          <h2>{{ category.category }}</h2>
          <div class="dyn--menu--infos">
            <div v-for="item in category.items" :key="item.name">{{ item.name }} - {{ item.price }}</div>
          </div>
        </div>
      </div>
  </div>
</template>

<script>
import BDD from '../BDD.js'
import { useRoute } from 'vue-router'

export default {
  name: 'RestaurantPage',
  setup () {
    const route = useRoute()
    const restaurant = BDD.find((restaurant) => restaurant.name === route.params.name)

    return {
      restaurant
    }
  }
}
</script>

<style lang="scss" scoped>
  #return {
    margin: 25px 0px;
    font-weight:400;
    cursor: pointer;
    font-size:20px;
    text-decoration: none;
    color:black;
    &:hover{
    font-weight: 900;
    }
  }

  .restaurantpage--section{
    display: flex;
    justify-content: flex-start;
    align-content: center;
    width:100%;
    height: 100vh;

    .dyn--restaurant--img{
        height: 250px;
        width: 100%;
        object-fit: cover;
    }

    ul{
      font-size:20px
    }

    li{
      font-size:20px
    }
  }

  .restaurant--menu--section{
    width: 50%;
    background: #f6f6f6;
    padding:50px;
    overflow: auto;
    
    .dyn--menu--infos{
        display: flex;
        gap:15px;
        flex-wrap: wrap;

        div{
          background-color: white;
          padding:20px;
        }
    }
  }

   .restaurant--infos--section{
    width: 50%;
    padding-right: 50px;
  }
</style>
