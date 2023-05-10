<template>
  <div>
    <ion-list>
      <ion-item>
        <ion-input
          v-model="nickname"
          label="Nickname"
          label-placement="stacked"
          fill="outline"
        >
        </ion-input>
      </ion-item>
      <ion-item>
        <ion-input
          v-model="email"
          label="Email"
          label-placement="stacked"
          fill="outline"
          type="email"
        >
        </ion-input>
      </ion-item>
      <ion-item>
        <ion-input
          v-model="password"
          label="Password"
          label-placement="stacked"
          fill="outline"
          type="password"
        >
        </ion-input>
      </ion-item>
    </ion-list>

    <ion-button @click="saveUser">log data</ion-button>
  </div>
</template>

<script>
import { ref } from "vue";
import { IonButton, IonItem, IonInput, IonList } from "@ionic/vue";

export default {
  components: { IonButton, IonItem, IonInput, IonList },

  setup() {
    const nickname = ref("");
    const email = ref("");
    const password = ref("");

    const saveUser = async () => {
      const userdata = {
        nickname: nickname.value,
        email: email.value,
        password: password.value,
      };

      console.log(JSON.stringify(userdata));
      try {
        const data = await fetch("http://localhost:3001/register", {
        method: "post",
        headers: {
          "content-type": "application/json",
        },
        body: JSON.stringify(userdata),
        mode: "cors",
      });
      } catch (err) {
        error.value = err.message;
        console.log(error.value);
      } 
    };

    return { nickname, email, password, saveUser };
  },
};
</script>

<style></style>
