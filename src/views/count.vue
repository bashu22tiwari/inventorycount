<template>
  <ion-page>
    <ion-header :translucent="true">
      <ion-toolbar>
        <ion-back-button slot="start" default-href="/"></ion-back-button>
        <ion-title>{{ $t("Count") }}</ion-title>
      </ion-toolbar>
    </ion-header>

    <ion-content :fullscreen="true">
      <div class="header">
        <div class="product-image">
          <img :src="product.mainImageUrl" />
        </div>
        <div class="product-info">
          <ion-item lines="none">
            <ion-label>
              <p class="overline">{{ product.productName }}</p>
              <h2>{{ product.sku }}</h2>
            </ion-label>
          </ion-item>

          <div class="product-features">
            <ion-item lines="none">
              <ion-chip>
                <ion-icon :icon="colorPaletteOutline" />
                <ion-label>{{ $filters.getFeature(product.featureHierarchy, '1/COLOR/') }}</ion-label>
              </ion-chip>
              <ion-chip>
                <ion-icon :icon="resize" />
                <ion-label>{{ $filters.getFeature(product.featureHierarchy, '1/SIZE/') }}</ion-label>
              </ion-chip>
            </ion-item>
          </div>
        </div>
      </div>
      <ion-item>
        <ion-label position="floating">{{ $t("Stock") }}</ion-label>
        <ion-input v-model="product.quantity"></ion-input>
      </ion-item>
      <ion-item lines="none">
        <ion-note id="stockCount">{{ $t("Enter the count of stock on the shelf.") }}</ion-note>
      </ion-item>

      <div class="action">
        <ion-button size="large" @click="updateProductInventoryCount()">
          <ion-icon :icon="saveOutline" slot="start" />{{
            $t("Save")
          }}</ion-button
        >
      </div>
    </ion-content>
  </ion-page>
</template>

<script lang="ts">
import {
  IonBackButton,
  IonButton,
  IonChip,
  IonContent,
  IonHeader,
  IonIcon,
  IonInput,
  IonItem,
  IonLabel,
  IonNote,
  IonPage,
  IonTitle,
  IonToolbar,
} from "@ionic/vue";
import { defineComponent } from "vue";
import { colorPaletteOutline, resize, saveOutline } from "ionicons/icons";
import { mapGetters, useStore } from "vuex";
import { showToast } from "@/utils";
import { translate } from "@/i18n";

export default defineComponent({
  name: "Count",
  components: {
    IonBackButton,
    IonButton,
    IonChip,
    IonContent,
    IonHeader,
    IonIcon,
    IonInput,
    IonItem,
    IonLabel,
    IonNote,
    IonPage,
    IonTitle,
    IonToolbar,
  },
  computed: {
    ...mapGetters({
      product: "product/getCurrent"
    })
  },
  methods: {
    updateProductInventoryCount() {
      if (this.product.quantity) {
        this.store.dispatch('product/updateInventoryCount', this.product);
      } else {
        showToast(translate("Enter the stock count for the product"))
      }
    }
  },
  setup() {
    const store = useStore();

    return {
      store,
      colorPaletteOutline,
      resize,
      saveOutline,
    };
  },
});
</script>

<style scoped>
.header {
  display: grid;
  grid: 1fr max-content / auto;
}

.product-image {
  grid-row: 1 / 3;
  grid-column: 1 / 2;
}

.product-image > img {
  width: 100%;
  height: 25vh;
  object-fit: cover;
}

.product-info {
  grid-row: 2 / 3;
  grid-column: 1 / 2;
  backdrop-filter: blur(20px);
  background: linear-gradient(
    180deg,
    rgba(196, 196, 196, 0) 0%,
    #ffffff 63.02%
  );
}

ion-item {
  --background: transparent;
}

#stockCount {
  margin-top: 8px;
}

.action {
  text-align: center;
}
</style>
