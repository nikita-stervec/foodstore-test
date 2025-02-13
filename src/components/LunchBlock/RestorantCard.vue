<template>
  <article class="restaurant-card">
    <header
        class="restaurant-header"
        :style="{ backgroundImage: `linear-gradient(rgba(0, 0, 0, 0.75), rgba(0, 0, 0, 0.5)), url(${restaurant.image})` }"
    >
      <div class="restaurant-info">
        <h3 class="restaurant-name">{{ restaurant.name }}</h3>
        <div class="restaurant-meta">
          <span>🕒 {{ restaurant.workingHours }}</span>
          <span>📍 {{ restaurant.address }}</span>
        </div>
      </div>
    </header>

    <ul class="lunch-menu">
      <li
          v-for="(lunch, index) in restaurant.lunches"
          :key="index"
          class="menu-item"
      >
        <span class="item-name">{{ lunch.name }}</span>
        <span class="item-price">{{ formatPrice(lunch.price) }}</span>
      </li>
    </ul>

    <footer class="restaurant-footer">
      Чек: <span class="total-price">{{ formatPrice(totalPrice) }}</span>
    </footer>
  </article>
</template>

<script>
export default {
  name: 'RestaurantCard',
  props: {
    restaurant: {
      type: Object,
      required: true
    }
  },
  computed: {
    totalPrice() {
      return this.restaurant.lunches.reduce((sum, lunch) => sum + lunch.price, 0);
    }
  },
  methods: {
    formatPrice(value) {
      return value.toLocaleString('ru-RU', {
        minimumFractionDigits: 2,
        maximumFractionDigits: 2
      }) + ' $';
    }
  }
};
</script>

<style lang="scss" scoped>
$color-accent: #27ae60;

.restaurant-card {
  background: white;
  border-radius: 12px;
  overflow: hidden;
  display: flex;
  flex-direction: column;
  transition: transform 0.2s ease;
  max-width: 350px;
  margin: 0 auto;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);

  &:hover {
    transform: translateY(-2px);
  }
}

.restaurant-header {
  padding: 1rem;
  min-height: 128px;
  display: flex;
  flex-direction: column;
  justify-content: flex-end;
  background-size: cover;
  background-position: center;
}

.restaurant-info {
  color: white;

  .restaurant-name {
    font-size: 1.75rem;
    margin: 0 0 0.5rem;
    line-height: 1.2;
  }
}

.restaurant-meta {
  display: flex;
  flex-direction: column;
  gap: 0.25rem;
  font-size: 0.9rem;
  opacity: 0.9;
}

.lunch-menu {
  list-style: none;
  padding: 1.5rem;
  margin: 0;
  flex-grow: 1;

  .menu-item {
    display: flex;
    justify-content: space-between;
    padding: 0.75rem 0;
    border-bottom: 1px solid #eee;

    &:last-child {
      border-bottom: none;
    }
  }

  .item-name {
    flex: 1;
    margin-right: 1rem;
  }

  .item-price {
    font-weight: 500;
    white-space: nowrap;
  }
}

.restaurant-footer {
  padding: 1.5rem;
  border-top: 2px solid #eee;
  text-align: right;
  font-size: 1.1rem;

  .total-price {
    color: $color-accent;
    font-weight: 600;
    margin-left: 0.5rem;
  }
}
</style>