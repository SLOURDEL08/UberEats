<template>
  <div class="restaurant--card">
    <div :style="changeBackground" class="restaurant--image">

    </div>
    <div class="restaurant--informations">
        <div class="top">
            <p class="name">{{ info_restaurant.name }}</p>
            <p class="note"><span>{{ info_restaurant.note }}</span></p>
        </div>
        <p class="time">{{ info_restaurant.drive_time }}</p>
    </div>
  </div>
</template>

<script>
import { computed } from 'vue'
export default {
name: "RestaurantCard",
props: {
    info_restaurant: Object
},
setup(props) {
    const changeBackground = computed(() => {
        return {
            backgroundImage: `url(${props.info_restaurant.image})`
        }
    })
    // return
    return {
        changeBackground
    }
}

}
</script>

<style lang="scss">
.restaurant--card {
    width: auto;
    height: 30vh;

    p {
        margin: 0px;
    }

    .restaurant--image {
        background-image: url("https://cn-geo1.uber.com/image-proc/resize/eats/format=webp/width=550/height=440/quality=70/srcb64=aHR0cHM6Ly9kMXJhbHNvZ25qbmczNy5jbG91ZGZyb250Lm5ldC8zNzg4MDJiMC1jNTI4LTQ4MjktYjBiNS0wY2M2NDBkZjYzY2QuanBlZw==");
        background-position: center;
        height: 70%;
        background-size: cover;
        transition: background-size 0.2s ease-in-out;
    }

    &:hover {
        p:first-child {
            text-decoration: underline;
        }

        .restaurant--image {
            background-size: 120%;

        }
    }

    .restaurant--informations {
        .top {
            margin: 20px 0px 0px 0px;
            display: flex;
            justify-content: space-between;
            align-content: center;

            .note {
                background-color: #eee;
                display: flex;
                justify-content: center;
                align-items: center;
                height: 30px;
                width: 30px;
                font-size: 0.8rem;
                border-radius: 50%;
            }
        }

        .time {
            font-size: 0.9rem;
        }
    }
}

</style>