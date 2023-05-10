<template>
  <ion-page>
    <div v-for="button in buttons" :key="button.nholes">
      <ion-button>{{ button.nholes }} hole</ion-button>
    </div>
  </ion-page>
</template>

<script>
import { ref } from "vue";
import { IonPage, IonButton } from "@ionic/vue";

import { Geolocation } from "@capacitor/geolocation";

export default {
  components: { IonPage, IonButton },

  setup() {
    // const allHoles = ref([])
    const buttons = ref([]);
    const error = ref(null);

    //GEOLOCATION
    const printCurrentPosition = async () => {
      const coordinates = await Geolocation.watchPosition(
        { enableHighAccuracy: true },
        (position) => {
          console.log("Current position:", position);
        }
      );
    };

    printCurrentPosition();

    const buttonData = async () => {
      try {
        const data = await fetch("http://localhost:3001/buttonData");

        if (!data.ok) {
          throw Error("no data available!");
        }
        buttons.value = await data.json();
        buttons.value = buttons.value.sort((a, b) => {
          return a.nholes - b.nholes;
        });
      } catch (err) {
        error.value = err.message;
        console.log(error.value);
      }
    };

    buttonData();

    return {
      buttons,
    };
  },
};
</script>

<style scoped>
#container {
  text-align: center;

  position: absolute;
  left: 0;
  right: 0;
  top: 50%;
  transform: translateY(-50%);
}

#container strong {
  font-size: 20px;
  line-height: 26px;
}

#container p {
  font-size: 16px;
  line-height: 22px;

  color: #8c8c8c;

  margin: 0;
}

#container a {
  text-decoration: none;
}
</style>
