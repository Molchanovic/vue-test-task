<script lang="ts" setup>
import { ref } from "vue";
import { userThings, allThings, IThings, maxUserThings } from "./products.data";
import ThingsCard from "./ThingsCard.vue";
const selectedUserThings = ref([]);
const selectedAllThing = ref(null);

function setUserThings(id: number) {
  const isAddedElemet = selectedUserThings.value.find(
    (card: IThings) => card.id === id
  );
  if (isAddedElemet) {
    const index = selectedUserThings.value.findIndex(
      (card: IThings) => card.id === id
    );
    selectedUserThings.value.splice(index, 1);
  } else {
    if (selectedUserThings.value.length < maxUserThings) {
      const targetElement = userThings.find((card: IThings) => card.id === id);
      selectedUserThings.value.push(targetElement);
    }
  }
}
function setAllThings(id: number) {
  const targetElement = allThings.find((card: IThings) => card.id === id);
  selectedAllThing.value = targetElement;
}
</script>
<template>
  <div class="selection-wrapper">
    <div class="selection-wrapper__header">
      <div class="selection-block selection-wrapper__header-block user-things">
        <div class="user-things__wrapper">
          <ThingsCard
            v-for="card of selectedUserThings"
            :key="card.id"
            :name="card.name"
            :id="card.id"
            @select="setUserThings"
          />
        </div>
        <div class="user-things__count">
          selected:
          <span>N</span>
          /
          <span>M</span>
        </div>
      </div>
      <div class="selection-block selection-wrapper__header-block all-things">
        <span v-if="!selectedAllThing"
          >SELECTED <br />
          ITEM</span
        >
        <div v-else class="all-things__wrapper">
          <ThingsCard
            :key="selectedAllThing.id"
            :name="selectedAllThing.name"
            :id="selectedAllThing.id"
          />
        </div>
      </div>
    </div>
    <div class="selection-wrapper__body">
      <div class="selection-block selection-wrapper__body-block">
        <ThingsCard
          v-for="card of userThings"
          :selectedCards="selectedUserThings"
          :key="card.id"
          :name="card.name"
          :id="card.id"
          @select="setUserThings"
        />
      </div>
      <div class="selection-block selection-wrapper__body-block">
        <ThingsCard
          v-for="card of allThings"
          :selectedCards="
            selectedAllThing ? [selectedAllThing] : selectedAllThing
          "
          :key="card.id"
          :name="card.name"
          :id="card.id"
          @select="setAllThings"
        />
      </div>
    </div>
  </div>
</template>
<style lang="scss" scoped>
.selection-wrapper {
  max-width: 1320px;
  color: #000;
  margin: 0px auto;
  &__header {
    display: flex;
    justify-content: space-between;
    gap: 40px;
    margin-bottom: 40px;
  }

  &__header-block {
    width: 400px;
    min-height: 200px;
  }

  &__body {
    display: flex;
    gap: 40px;
  }

  &__body-block {
    flex: 1 1 45%;
    display: flex;
    flex-wrap: wrap;
    gap: 20px;
  }
}
.selection-block {
  border: 5px solid #000;
  padding: 10px;
}

.user-things {
  font-size: 20px;
  line-height: 26px;
  display: flex;
  flex-direction: column;
  &__wrapper {
    display: flex;
    flex-wrap: wrap;
    gap: 20px;
    margin-bottom: 20px;
  }

  &__count {
    margin-top: auto;
  }
}

.all-things {
  font-size: 28px;
  line-height: 36px;
  font-weight: 500;
  display: flex;
  align-items: center;
  justify-content: center;
}
</style>
