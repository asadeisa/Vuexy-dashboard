<script setup>
import { ref } from 'vue'
import image1 from '@images/eCommerce/iphone.png'
import image2 from '@images/eCommerce/1.png'
import image3 from '@images/eCommerce/nike.png'

const products = ref([
  {
    id: 1,
    name: 'iphone',
    price: 120,
    image: image1,
    category: 'Electronics',
  },
  {
    id: 2,
    name: 'Smart Watch',
    price: 250,
    image: image2,
    category: 'Gadgets',
  },
  {
    id: 3,
    name: 'Running Shoes',
    price: 90,
    image: image3,
    category: 'Fashion',
  },
])

const search = ref('')
const range  = ref([50, 500])
</script>

<template>
  <VContainer>
    <VRow>
      <!-- Sidebar filters -->
      <VCol
        cols="12"
        md="3"
      >
        <VCard class="pa-4">
          <VTextField
            v-model="search"
            label="Search products"
            prepend-inner-icon="tabler-search"
          />

          <VSelect
            label="Category"
            :items="['Electronics', 'Gadgets', 'Fashion']"
            clearable
          />

          <VRangeSlider
            v-model="range"
            :min="50"
            :max="500"
            label="Price Range"
            thumb-label="always"
          />
        </VCard>
      </VCol>

      <!-- Product grid -->
      <VCol
        cols="12"
        md="9"
      >
        <VRow>
          <VCol
            v-for="product in products"
            :key="product.id"
            cols="12"
            sm="6"
            md="4"
          >
            <VCard class="h-full">
              <VImg
                :src=" product.image"
                height="180"
              />
              <VCardItem>
                <VCardTitle>{{ product.name }}</VCardTitle>
                <VCardSubtitle>${{ product.price }}</VCardSubtitle>
              </VCardItem>
              <VCardActions>
                <VBtn
                  color="primary"
                  block
                >
                  <VIcon
                    icon="tabler-shopping-cart-plus"
                    start
                  />
                  Add to Cart
                </VBtn>
              </VCardActions>
            </VCard>
          </VCol>
        </VRow>
      </VCol>
    </VRow>
  </VContainer>
</template>
