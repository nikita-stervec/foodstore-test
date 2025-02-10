<template>
  <div class="lunchBlock">
    <div class="lunchBlock__header">
      <h2>Business-Lunches in da Vitebsk (Hood)</h2>
      <div>
        <button @click="prevPage" :disabled="currentPage === 0">Назад</button>
        <button @click="nextPage" :disabled="currentPage === totalPages - 1">Вперед</button>
      </div>
    </div>
    <div class="lunchBlock__main">
      <div v-for="restaurant in visibleRestaurants" :key="restaurant.id" class="restaurant">
        <div
          class="restaurant__header"
          :style="{ backgroundImage: `linear-gradient(rgba(0, 0, 0, 0.75), rgba(0, 0, 0, 0.5)), url(${restaurant.image})`, borderTopLeftRadius: `12px`, borderTopRightRadius: `12px` }"
        >
          <div class="restaurant__info">
            <h2>{{ restaurant.name }}</h2>
            <span>🕒 {{ restaurant.workingHours }}</span>
            <span>📍 {{ restaurant.address }}</span>
          </div>
        </div>
        <ul class="lunch-list">
          <li v-for="(lunch, index) in restaurant.lunches" :key="index">
            <span class="lunch-name">{{ lunch.name }}</span>
            <span class="lunch-price">{{ lunch.price.toFixed(2) }} р</span>
          </li>
        </ul>
        <div class="total-price">
          Итого: {{ calculateTotal(restaurant.lunches).toFixed(2) }} р
        </div>
      </div>
    </div>
    <div class="lunchBlock__bottom">
      <h3>Страница {{ currentPage + 1 }} из {{ totalPages }}</h3>
      <img class="lunchBlock__bottom-image" src="../../assets/bg-icon.svg" alt="bg">
    </div>
  </div>
</template>

<script>
import restaurantsData from '@/assets/restaurants.json';

export default {
  name: 'LunchBlock',
  data() {
    return {
      restaurants: restaurantsData,
      currentPage: 0,
      itemsPerPage: 4 
    };
  },
  computed: {
    totalPages() {
      return Math.ceil(this.restaurants.length / this.itemsPerPage);
    },
    visibleRestaurants() {
      const start = this.currentPage * this.itemsPerPage;
      return this.restaurants.slice(start, start + this.itemsPerPage);
    }
  },
  methods: {
    nextPage() {
      if (this.currentPage < this.totalPages - 1) this.currentPage++;
    },
    prevPage() {
      if (this.currentPage > 0) this.currentPage--;
    },
    calculateTotal(lunches) {
      return lunches.reduce((sum, lunch) => sum + lunch.price, 0);
    }
  }
};
</script>

<style lang="scss" scoped>
.lunchBlock {
  border-radius: 12px;
  max-width: 1200px;
  margin: 0 auto;
  padding: 20px;
  background: #f5f5f5;
}

.lunchBlock__header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 30px;
  padding: 15px;
  background: white;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.lunchBlock__main {
  display: grid;
  gap: 20px;
  min-height: 500px;

  grid-template-columns: 1fr;


  @media (min-width: 768px) {
    grid-template-columns: repeat(2, 1fr);
  }

  @media (min-width: 1024px) {
    grid-template-columns: repeat(4, 1fr);
  }
}

.restaurant {
  background: white;
  border-radius: 12px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  transition: .5s;

  &__header {
    margin-bottom: 15px;
    border-bottom: 1px solid #eee;
    padding-bottom: 10px;

    h2 {
      margin: 0 0 8px 0;
      color: #ffffff;
      font-size: 1.5em;
    }
  }

  &__info {
    max-height: 96px;
    height: 96px;
    padding: 10px;
    display: flex;
    flex-direction: column;
    gap: 5px;
    font-size: 0.9em;
    color: #ffffff;
  }
}

.restaurant:hover {
  transition: .3s;
  background: #f7f5f5;
}

.lunch-list {
  list-style: none;
  padding: 20px;
  margin: 0 0 15px 0;

  li {
    display: flex;
    justify-content: space-between;
    align-items: center; 
    padding: 8px 0;
    border-bottom: 1px solid #eee;

    &:last-child {
      border-bottom: none;
    }

    .lunch-name {
      flex: 1; 
      margin-right: 10px; 
    }

    .lunch-price {
      flex-shrink: 0; 
      font-weight: 500;
      color: #2c3e50;
    }
  }
}

.total-price {
  padding: 20px;
  text-align: right;
  font-weight: bold;
  color: #27ae60;
  padding: 10px;
  border-top: 2px solid #eee;
}

button {
  min-width: 84px;
  width: 84px;
  padding: 8px 16px;
  margin-left: 10px;
  border: none;
  border-radius: 4px;
  background: #3498db;
  color: white;
  cursor: pointer;
  transition: background 0.3s;

  &:disabled {
    background: #bdc3c7;
    cursor: not-allowed;
  }

  &:hover:not(:disabled) {
    background: #2980b9;
  }
}

.lunchBlock__bottom {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  overflow: hidden;
}

</style>