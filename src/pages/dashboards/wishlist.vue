<script setup>
import { ref } from 'vue'
import image1 from '@images/eCommerce/iphone.png'
import image2 from '@images/eCommerce/1.png'
import image3 from '@images/eCommerce/nike.png'

const wishlist = ref([
  {
    id: 1,
    name: 'iphone',
    price: 120,
    image: image1,
    inStock: true,
  },
  {
    id: 2,
    name: 'Smart Watch',
    price: 250,
    image: image2,
    inStock: false,
  },
  {
    id: 3,
    name: 'Running Shoes',
    price: 90,
    image: image3,
    inStock: true,
  },
])

function moveToCart(productId) {
  // here you'd normally add to cart store
  const product = wishlist.value.find(p => p.id === productId)
  if (product) {
    alert(`Moved "${product.name}" to cart`)
    removeFromWishlist(productId)
  }
}

function removeFromWishlist(productId) {
  wishlist.value = wishlist.value.filter(p => p.id !== productId)
}
</script>

<template>
  <VContainer class="wishlist-page">
    <VCard>
      <VCardItem>
        <VCardTitle>My Wishlist</VCardTitle>
        <VCardSubtitle>Saved products for later</VCardSubtitle>
      </VCardItem>

      <VCardText>
        <VTable class="wishlist-table">
          <thead>
            <tr>
              <th class="h-text">
                Product
              </th>
              <th class="h-text">
                Status
              </th>
              <th class="h-text">
                Price
              </th>
              <th class="text-center h-text">
                Actions
              </th>
            </tr>
          </thead>
          <tbody>
            <tr
              v-for="product in wishlist"
              :key="product.id"
            >
              <!-- Product cell -->
              <td>
                <div class="d-flex flex-wrap items-center  gap-3 ">
                  <div class="rounded image-in-cart">
                    <img
                      :src="product.image"
                      width="60"
                      height="60"
                    >
                  </div>
                 
                  <span class="font-medium font-weight-medium">{{ product.name }}</span>
                </div>
              </td>

              <!-- Stock status -->
              <td>
                <VChip
                  :color="product.inStock ? 'success' : 'error'"
                  size="small"
                  class="font-weight-medium"
                >
                  {{ product.inStock ? 'In Stock' : 'Out of Stock' }}
                </VChip>
              </td>

              <!-- Price -->
              <td>
                <span class="font-semibold font-weight-medium">${{ product.price }}</span>
              </td>

              <!-- Actions -->
              <td class="text-center ">
                <div class=" d-flex">
                  <VBtn
                    color="primary"
                    variant="tonal"
                    size="small"
                    class="me-2 font-weight-medium small-icon-button"
                    :disabled="!product.inStock"
                    @click="moveToCart(product.id)"
                  >
                    <VIcon
                      icon="tabler-shopping-cart-plus"
                      start
                      class="font-weight-medium"
                    />
                    <span class="h-in-sm">
  
                      Move to Cart
                    </span>
                  </VBtn>
  
                  <VBtn
                    color="error"
                    variant="text"
                    size="small"
                    class="small-icon-button"
                    @click="removeFromWishlist(product.id)"
                  >
                    <VIcon icon="tabler-trash" />
                  </VBtn>
                </div>
              </td>
            </tr>
          </tbody>
        </VTable>

        <!-- Empty state -->
        <div
          v-if="wishlist.length === 0"
          class="text-center py-10 text-gray-500"
        >
          <VIcon
            icon="tabler-heart-off"
            size="40"
            class="mb-2"
          />
          <p>No items in your wishlist.</p>
        </div>
      </VCardText>
    </VCard>
  </VContainer>
</template>

