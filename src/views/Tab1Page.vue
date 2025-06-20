<template>
  <ion-page>
    <ion-header>
      <ion-toolbar>
        <ion-title>Location</ion-title>
      </ion-toolbar>
    </ion-header>
    <ion-content :fullscreen="true">
      <ion-header collapse="condense">
        <ion-toolbar>
          <ion-title size="large">Location</ion-title>
        </ion-toolbar>
      </ion-header>

      <div class="location-container">
        <ion-button @click="getCurrentPosition">Get Current Location</ion-button>
        
        <ion-card v-if="location">
          <ion-card-header>
            <ion-card-title>Current Location</ion-card-title>
          </ion-card-header>
          <ion-card-content>
            <p>Latitude: {{ location.latitude }}</p>
            <p>Longitude: {{ location.longitude }}</p>
            <p v-if="location.accuracy">Accuracy: ±{{ location.accuracy }}m</p>
          </ion-card-content>
        </ion-card>

        <ion-text color="danger" v-if="error">
          {{ error }}
        </ion-text>
      </div>
    </ion-content>
  </ion-page>
</template>

<script setup lang="ts">
import { ref } from 'vue';
import { Geolocation } from '@capacitor/geolocation';
import { 
  IonPage, 
  IonHeader, 
  IonToolbar, 
  IonTitle, 
  IonContent,
  IonButton,
  IonCard,
  IonCardHeader,
  IonCardTitle,
  IonCardContent,
  IonText
} from '@ionic/vue';

const location = ref<{ latitude: number; longitude: number; accuracy?: number } | null>(null);
const error = ref<string | null>(null);

const getCurrentPosition = async () => {
  try {
    const position = await Geolocation.getCurrentPosition();
    location.value = {
      latitude: position.coords.latitude,
      longitude: position.coords.longitude,
      accuracy: position.coords.accuracy
    };
    error.value = null;
  } catch (e) {
    error.value = 'Error getting location. Please ensure location permissions are enabled.';
    console.error(e);
  }
};
</script>

<style scoped>
.location-container {
  padding: 20px;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 20px;
}

ion-card {
  width: 100%;
  max-width: 400px;
}
</style>
