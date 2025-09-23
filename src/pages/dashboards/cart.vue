<script setup>
import { ref, computed } from 'vue'
import image1 from '@images/eCommerce/iphone.png'
import image2 from '@images/eCommerce/1.png'


const cart = ref([
  {
    id: 1,
    name: 'iphone',
    price: 120,
    qty: 1,
    image: image1,
  },
  {
    id: 2,
    name: 'Smart Watch',
    price: 250,
    qty: 2,
    image: image2,
  },
])

// Update quantity
function updateQty(productId, amount) {
  const item = cart.value.find(p => p.id === productId)
  if (item) {
    item.qty = Math.max(1, item.qty + amount)
  }
}

// Remove item
function removeFromCart(productId) {
  cart.value = cart.value.filter(p => p.id !== productId)
}

// Totals
const subtotal = computed(() =>
  cart.value.reduce((acc, item) => acc + item.price * item.qty, 0),
)

const tax = computed(() => subtotal.value * 0.1) // example 10% tax
const total = computed(() => subtotal.value + tax.value)
</script>

<template>
  <VContainer class="cart-page">
    <VRow>
      <VCol
        cols="12"
        md="8"
      >
        <VCard>
          <VCardItem>
            <VCardTitle class="text-h6 text-md-h5">
              Shopping Cart
            </VCardTitle>
          </VCardItem>

          <VCardText>
            <VTable class="cart-table">
              <thead>
                <tr>
                  <th class="h-text">
                    Product
                  </th>
                  <th class="text-center h-text">
                    Quantity
                  </th>
                  <th class="text-center h-text">
                    Price
                  </th>
                  <th class="text-center h-text">
                    Subtotal
                  </th>
                  <th class="text-center h-text">
                    Actions
                  </th>
                </tr>
              </thead>
              <tbody>
                <tr
                  v-for="item in cart"
                  :key="item.id"
                >
                  <!-- Product -->
                  <td>
                    <div class="d-flex align-center gap-3">
                      <VImg
                        :src="item.image"
                        width="60"
                        height="60"
                        cover
                        class="rounded image-in-cart"
                      />
                      <span class="font-weight-medium">{{ item.name }}</span>
                    </div>
                  </td>

                  <!-- Quantity -->
                  <td class="text-center">
                    <div class="d-flex align-center justify-center">
                      <VBtn
                        size="small"
                        variant="outlined"
                        class="quantity-btn"
                        icon="tabler-minus"
                        @click="updateQty(item.id, -1)"
                      />
                      <span class="mx-2 font-weight-medium">{{ item.qty }}</span>
                      <VBtn
                        size="small"
                        variant="outlined"
                        icon="tabler-plus"
                        class="quantity-btn"
                        @click="updateQty(item.id, 1)"
                      />
                    </div>
                  </td>

                  <!-- Price -->
                  <td class="text-center font-weight-medium">
                    ${{ item.price }}
                  </td>

                  <!-- Subtotal -->
                  <td class="text-center font-weight-medium">
                    ${{ (item.price * item.qty).toFixed(2) }}
                  </td>

                  <!-- Actions -->
                  <td class="text-center">
                    <VBtn
                      color="error"
                      size="small"
                      variant="text"
                      @click="removeFromCart(item.id)"
                    >
                      <VIcon icon="tabler-trash" />
                    </VBtn>
                  </td>
                </tr>
              </tbody>
            </VTable>

          

            <!-- Empty cart -->
            <div
              v-if="cart.length === 0"
              class="text-center py-10 grey--text"
            >
              <VIcon
                icon="tabler-shopping-cart-off"
                size="40"
                class="mb-2"
              />
              <p>Your cart is empty.</p>
            </div>
          </VCardText>
        </VCard>
      </VCol>

      <!-- Cart Summary -->
      <VCol
        cols="12"
        md="4"
      >
        <VCard>
          <VCardItem>
            <VCardTitle class="text-h6 text-md-h5">
              Order Summary
            </VCardTitle>
          </VCardItem>

          <VCardText>
            <div class="d-flex justify-space-between mb-2">
              <span class="h-text">Subtotal:</span>
              <span class="h-text">${{ subtotal.toFixed(2) }}</span>
            </div>
            <div class="d-flex justify-space-between mb-2">
              <span class="h-text">Tax (10%):</span>
              <span class="h-text">${{ tax.toFixed(2) }}</span>
            </div>
            <div class="d-flex justify-space-between font-weight-bold text-subtitle-1 mb-4">
              <span class="h-text">Total:</span>
              <span class="h-text">${{ total.toFixed(2) }}</span>
            </div>

            <VBtn
              color="primary"
              block
              class="h-text"
            >
              <VIcon
                icon="tabler-credit-card"
                start
              /> Proceed to Checkout
            </VBtn>
          </VCardText>
        </VCard>
      </VCol>
    </VRow>
  </VContainer>
</template>
