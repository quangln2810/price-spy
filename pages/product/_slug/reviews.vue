<template>
  <div class="review-container">
    <div class="review-left">
      <h1 class="title">Reviews</h1>
      <div class="sorted-by">Sorted by date</div>
      <div v-for="review in reviews.docs" :key="review._id" class="review-item">
        <div class="user">{{ review.username }}</div>
        <div class="header">
          <div class="stars">
            <div class="star" :style="{ width: (review.rating / 5) * 100 + '%' }">
              <i class="fa fa-star" />
            </div>
          </div>
          <div class="date">{{ review.date | date | dateToNow }}</div>
        </div>
        <div>
          {{ review.content }}
        </div>
      </div>
      <Paginator v-bind="reviews" style="margin-top: 1rem" />
    </div>
    <div class="review-right">
      <h1 class="title">Product rating</h1>
      <div class="rating">
        <h1>{{ +product.rating.toFixed(1) }}</h1>
        <div class="stars lg">
          <div class="star" :style="{ width: (product.rating / 5) * 100 + '%' }">
            <i class="fa fa-star" />
          </div>
        </div>
      </div>
      <div>{{ product.reviewCount }} reviews</div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    product: {
      type: Object,
      default: () => ({}),
    },
    category: {
      type: Object,
      default: () => ({}),
    },
  },
  data() {
    return {
      reviews: [],
    };
  },
  async fetch() {
    this.reviews = await this.$axios.$get(`/product/${this.product._id}/reviews`, {
      params: { page: this.$route.query.page },
    });
  },
  watch: {
    $route(to, from) {
      if (to.path === from.path) {
        this.$fetch();
      }
    },
  },
};
</script>

<style lang="scss" scoped>
.sorted-by {
  margin-bottom: 2em;
}
.review-container {
  display: grid;
  grid-template-columns: 70% 30%;
  column-gap: 0.5rem;
}
.review-item {
  min-height: 5rem;
  .user {
    font-weight: bold;
    margin-bottom: 0.5rem;
  }
  .header {
    display: flex;
    margin-bottom: 0.5em;
    .date {
      &::before {
        content: '|';
        margin-left: 0.5rem;
        margin-right: 0.5rem;
        position: relative;
        top: -1px;
      }
    }
  }
}
.rating {
  display: flex;
  align-items: center;
  .stars {
    margin-left: 0.5rem;
  }
}
</style>
