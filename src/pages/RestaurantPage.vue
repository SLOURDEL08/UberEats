<template>
  <div class="restaurantpage--section">
      <div class="restaurant--infos--section">
        <RouterLink to="/">
        <div class="flex--backhome">
          <img src="/arrowback.png" class="arrowback--restaurantpage"/>
          <div id="return">Retour à la home</div>
          </div>
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
            <div class="dyn--flexmenu" v-for="item in category.items" :key="item.name">
              <img class="img--item" :src="item.image"/>
                <div class="infos--item">
                  <span>{{ item.name }}</span>
                  <p>{{ item.price }}<b> $</b></p>
                </div>
              </div>
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
        gap:30px;
        flex-wrap: wrap;
        border-radius: 50%;

        .dyn--flexmenu{
          width: 250px;
            display: flex;
            align-items: center;
                      background-color: white;
                      height: 100px;
                      border-radius: 10px;

        }

        .img--item{
          width: 40%;
          height: 100% ;
          object-fit: cover;
          border-radius: 10px 0px 0px 10px;
          background-color:#3e3e3e;

        }

         .infos--item{
          padding:20px;
           
           span{
            font-weight:500;
            margin:0px;
                        font-size:16px;
              padding:0px;
           }

           p{
            font-size:14px;
            line-height: 1.6;
            font-weight:500;
              margin:10px 0px 0px 0px;
              padding:0px;
              font-style: italic;
              color:grey;

          
           }

           b{
              font-weight:500;
              font-size:14px;
              font-style:italic;
              margin-bottom:0px;
              margin:0px;
              padding:0px;
              color:black
            }
        }

       
    }
  }

   .restaurant--infos--section{
    width: 50%;
    padding-right: 50px;
  }

  .arrowback--restaurantpage{
    width: 15px;
    height: 15px;
    margin-right: 10px;
  }

  .flex--backhome{
    display: flex;
    justify-content: flex-start;
    align-items: center;
  }
</style>
