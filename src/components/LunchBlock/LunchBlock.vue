<template>
  <div class="lunch-block">
    <header class="lunch-block__header">
      <h2 class="header-title">
        Business Lunches in Vitebsk <span class="subtitle">(Hood)</span>
      </h2>
      <div class="pagination-controls">
        <button
            class="pagination-button"
            @click="currentPage--"
            :disabled="isFirstPage"
            aria-label="Previous page"
        >
          &lt;
        </button>
        <button
            class="pagination-button"
            @click="currentPage++"
            :disabled="isLastPage"
            aria-label="Next page"
        >
          &gt;
        </button>
      </div>
    </header>

    <main class="lunch-block__content">
      <transition :name="transitionEffect" mode="out-in">
        <div class="restaurants-grid" :key="currentPage">
          <RestaurantCard
              v-for="restaurant in paginatedRestaurants"
              :key="restaurant.id"
              :restaurant="restaurant"
          />
        </div>
      </transition>
    </main>

    <footer class="lunch-block__footer">
      <div class="page-indicator">
        Page {{ currentPage + 1 }} of {{ totalPages }}
      </div>
      <div class="background-container">
        <img
            class="footer-decoration"
            src="@/assets/bg-icon.svg"
            alt="Decorative background pattern"
        >
      </div>
    </footer>
  </div>
</template>

<script>
import RestaurantCard from '@/components/LunchBlock/RestorantCard.vue';
import restaurantsData from '@/assets/restaurants.json';

const ITEMS_PER_PAGE = 4;

// Функция для корректного преобразования цен
const normalizePrices = (restaurants) => restaurants;

export default {
  name: 'LunchBlock',
  components: { RestaurantCard },
  data: () => ({
    restaurants: normalizePrices(restaurantsData), // Используем нормализованные данные
    currentPage: 0,
    transitionEffect: 'slide-next'
  }),
  computed: {
    totalPages() {
      return Math.ceil(this.restaurants.length / ITEMS_PER_PAGE);
    },
    paginatedRestaurants() {
      const start = this.currentPage * ITEMS_PER_PAGE;
      return this.restaurants.slice(start, start + ITEMS_PER_PAGE);
    },
    isFirstPage() {
      return this.currentPage === 0;
    },
    isLastPage() {
      return this.currentPage === this.totalPages - 1;
    }
  },
  watch: {
    currentPage(newVal, oldVal) {
      this.transitionEffect = newVal > oldVal ? 'slide-next' : 'slide-prev';
    }
  }
};
</script>

<style lang="scss" scoped>
$color-primary: rgb(119, 90, 55);
$color-accent: rgb(150, 128, 102);
$color-disabled: rgb(229, 225, 221);
$border-radius: 12px;

.lunch-block {
  font-family: 'Fira Sans', sans-serif;
  max-width: 1400px;
  margin: 0 auto;
  padding: 1rem;
  background: #f5f5f5;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  border-radius: $border-radius;
  position: relative;
  overflow: hidden;

  &__header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 1rem;
    margin-bottom: 2rem;
    background: white;
    border-radius: $border-radius;
    position: relative;
    z-index: 2;
  }

  &__content {
    padding: 0 2rem;
    min-height: 500px;
    position: relative;
  }

  &__footer {
    width: 100vw;
    position: relative;
    left: 50%;
    transform: translateX(-50%);
    padding: 2rem 0;
  }
}

.header-title {
  color: $color-primary;
  font-weight: 400;
  margin: 0;

  .subtitle {
    font-weight: 300;
  }
}

.restaurants-grid {
  display: grid;
  gap: 1.25rem;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  justify-content: center;
}

.pagination-controls {
  display: flex;
  gap: 0.5rem;
}

.pagination-button {
  width: 4rem;
  padding: 0.75rem;
  border: none;
  border-radius: 4px;
  background: $color-accent;
  color: white;
  cursor: pointer;
  transition: background 0.3s ease;

  &:disabled {
    background: $color-disabled;
    cursor: not-allowed;
  }

  &:hover:not(:disabled) {
    background: darken($color-accent, 10%);
  }
}

.page-indicator {
  color: darken($color-primary, 10%);
  font-weight: 500;
  margin-bottom: 1.5rem;
  text-align: center;
}

.background-container {
  max-width: 1500px;
  margin: 0 auto;
  height: 137px;
  overflow: hidden;
}

.footer-decoration {
  width: 100%;
  height: 100%;
  object-fit: cover;
  object-position: center;
  opacity: 0.8;
}

.slide-next-enter-active,
.slide-next-leave-active,
.slide-prev-enter-active,
.slide-prev-leave-active {
  transition: all 0.3s ease;
}

.slide-next-enter-from {
  transform: translateX(100%);
  opacity: 0;
}

.slide-next-leave-to {
  transform: translateX(-100%);
  opacity: 0;
}

.slide-prev-enter-from {
  transform: translateX(-100%);
  opacity: 0;
}

.slide-prev-leave-to {
  transform: translateX(100%);
  opacity: 0;
}
</style>