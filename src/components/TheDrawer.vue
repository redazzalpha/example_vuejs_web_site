<template>
  <VNavigationDrawer
    :model-value="props.drawer"
    @update:model-value="emit('update:drawer', $event)"
    :location="appStore.navigationSide ? 'right' : 'left'"
    temporary
    color="color-black-light-const"
    width="300"
  >
    <DrawerList :items="drawerListItem" :value-badge="appStore.cart.length" />
  </VNavigationDrawer>
</template>

<script lang="ts" setup>
import DrawerList from "@/components/DrawerList.vue";
import { useAppStore } from "@/store/app";
import { drawerListItem } from "@/utils/drawerlist";

const appStore = useAppStore();

// #region props
interface Props {
  drawer: boolean;
}
const props = defineProps<Props>();
// #endregion

// #region emits
interface Emits {
  (event: "update:drawer", value: boolean): void;
}
const emit = defineEmits<Emits>();
// #endregion
</script>
