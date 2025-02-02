<template>
  <div
    class="description_container d-flex flex-column elevation-10 mx-5 mx-md-auto"
    :class="`bg-color-white`"
  >
    <VBtn
      class="description_back-button text-h4 pa-0"
      variant="plain"
      :ripple="false"
      @click="$router.back"
    >
      <VIcon>mdi-arrow-left-thick</VIcon>
    </VBtn>
    <h2 class="description_title text-color-black text-h5 text-lg-h4">
      {{ appStore.currentItem.label }}
    </h2>
    <div
      class="description_content d-flex flex-column flex-md-row justify-center"
    >
      <VImg
        class="description_content_img ma-5 flex-grow-0"
        :src="current.img"
        :width="imgWidth"
        cover
      ></VImg>
      <div
        class="description_content_text d-flex flex-column d-block ma-5 flex-shrink-1 my-md-auto"
      >
        <p class="text-color-black">
          <strong> Description: </strong>
          {{ current.description }} <br />
          <strong> Prix: </strong>
          {{ formatNumber(appStore.currentItem.price) }}&euro; <br />
          <strong> Note: </strong>
          <span>
            <template v-for="count in 6" :key="count">
              <VIcon
                :icon="
                  appStore.currentItem.score >= count
                    ? 'mdi-star'
                    : 'mdi-star-outline'
                "
                variant="plain"
                color="color-orange"
              />
            </template>
          </span>
          <br />
          <strong> Category: </strong>
          {{ appStore.currentItem.category }} <br />
          <strong> Nouveau produit: </strong>
          <span
            :class="
              appStore.currentItem.isNew
                ? 'text-color-success-const'
                : 'text-color-error-const'
            "
          >
            {{ appStore.currentItem.isNew ? "oui" : "non" }}
          </span>
          <br />
          <strong> Produit populaire: </strong>
          <span
            :class="
              appStore.currentItem.isPopular
                ? 'text-color-success-const'
                : 'text-color-error-const'
            "
          >
            {{ appStore.currentItem.isPopular ? "oui" : "non" }} <br />
          </span>
        </p>
        <VBtn
          class="description_content_text_btn text-color-black align-self-center ma-10"
          size="large"
          rounded="xl"
          width="320"
          variant="elevated"
          color="color-orange"
          @click.prevent="appStore.openDialog(appStore.currentItem)"
        >
          Ajouter au panier
          <VIcon class="mx-2">mdi-cart-plus</VIcon>
        </VBtn>
      </div>
    </div>
  </div>

  <DialogSlider
    v-model:dialog="appStore.modelDialog"
    v-model:slider="appStore.modelSlider"
    :product="appStore.currentItem"
    :max="20"
    :min="1"
    @on-validate="props.action"
  />
</template>

<script lang="ts" setup>
import vuetify from "@/plugins/vuetify";
import router from "@/router";
import { useAppStore } from "@/store/app";
import type { Product } from "@/utils/types";
import { computed, onBeforeMount } from "vue";
import DialogSlider from "./DialogSlider.vue";

const appStore = useAppStore();

//#region variables
//#endregion

//#region props
interface Props {
  action: (id: number, quantity: number, totalPrice: string) => void;
}
const props = defineProps<Props>();
//#endregion

//#region computed
const current = computed<Product>(() => {
  return appStore.productCurrent as Product;
});

const imgWidth = computed(() => {
  if (vuetify.display.sm.value) return "300";
  return vuetify.display.md.value ||
    vuetify.display.lg.value ||
    vuetify.display.xl.value
    ? "500"
    : "";
});
//#endregion

//#region methods
function formatNumber(value: number): string {
  let valueStr = value.toFixed(2);
  const index = valueStr.indexOf(".");
  if (index > -1) {
    const numOfDigits = valueStr.length - index;
    if (numOfDigits == 1) valueStr += "0";
  }
  return valueStr;
}
//#endregion

//#region  hooks
onBeforeMount(() => {
  const index: number = Number(router.currentRoute.value.params.id) - 1;
  appStore.productCurrent = appStore.products[index];
});
//#endregion
</script>

<style lang="scss" scoped>
.description_container {
  position: relative;
  justify-content: center;
  align-content: center;
  border-radius: 10px;
  /* background-color: v-bind("baseColor.whiteDark"); */
  margin-top: 100px;
  max-width: 1000px;
  .description_title {
    margin-top: 40px !important;
  }
  .description_back-button {
    position: absolute;
    top: 5px;
    left: 0;
  }
  .description_content {
    &_img {
      border-radius: 10px;
      margin: auto;
      max-height: 300px;
    }
    &_text {
      max-width: 900px;
    }
  }
}
</style>
