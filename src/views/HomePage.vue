<template>
  <ion-page>
    <ion-header :translucent="true">
      <ion-toolbar>
        <ion-title>Crypto Prices</ion-title>
      </ion-toolbar>
    </ion-header>

    <ion-content :fullscreen="true">
      <div class="refresh-container">
        <ion-button @click="fetchData" color="primary">Refresh</ion-button>
      </div>

      <ion-list>
        <ion-item
          v-for="crypto in cryptos"
          :key="crypto.rank"
          class="crypto-item"
        >
          <ion-grid>
            <ion-row>
              <ion-col size="2" class="rank-col">
                <div class="rank-label">Rank</div>
                <div class="rank-number">{{ crypto.rank }}</div>
              </ion-col>
              <ion-col size="6" class="middle-col">
                <div class="name">{{ crypto.name }}</div>
                <div class="symbol">{{ crypto.symbol }}</div>
              </ion-col>
              <ion-col size="4" class="right-col">
                <div class="usd-label">USD</div>
                <div class="price">{{ crypto.price_usd }}</div>
              </ion-col>
            </ion-row>
          </ion-grid>
        </ion-item>
      </ion-list>
    </ion-content>
  </ion-page>
</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue';
import {
  IonContent,
  IonHeader,
  IonPage,
  IonTitle,
  IonToolbar,
  IonButton,
  IonList,
  IonItem,
  IonGrid,
  IonRow,
  IonCol,
} from '@ionic/vue';
import EndPointAccess from '@/services/EndPointAccess/index';

interface Crypto {
  rank: number;
  name: string;
  symbol: string;
  price_usd: string;
}

const api = new EndPointAccess('https://api.coinlore.net/api/tickers/');
const cryptos = ref<Crypto[]>([]);

const fetchData = async () => {
  try {
    const res = await api.getRes<any>();
    cryptos.value = res.data.data.map((item: any) => ({
      rank: item.rank,
      name: item.name,
      symbol: item.symbol,
      price_usd: item.price_usd,
    }));
  } catch (error) {
    console.error('Error fetching data:', error);
  }
};

onMounted(fetchData);
</script>

<style scoped>
.refresh-container {
  text-align: center;
  margin: 20px 0;
}

.crypto-item {
  --background: #fff8dc;
  --border-color: #ddd;
  --border-width: 1px 0;
}

.rank-col {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  color: #000;
}

.rank-label {
  font-size: 12px;
  color: #666;
  margin-bottom: 4px;
}

.rank-number {
  font-size: 18px;
  font-weight: bold;
}

.middle-col {
  display: flex;
  flex-direction: column;
  justify-content: center;
}

.name {
  font-size: 14px;
  color: #666;
}

.symbol {
  font-size: 18px;
  font-weight: bold;
  color: #000;
}

.right-col {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: flex-end;
}

.usd-label {
  font-size: 14px;
  color: #666;
}

.price {
  font-size: 18px;
  font-weight: bold;
  color: #000;
}
</style>
