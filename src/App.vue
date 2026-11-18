<script setup>
import { ref, computed } from 'vue'

// بيانات المنتجات
const products = ref([
  { id: 1, name: 'Wireless Headphones', category: 'Electronics', price: 120, image: 'https://images.unsplash.com/photo-1505740420928-5e560c06d30e?w=400&q=80' },
  { id: 2, name: 'Smart Watch Pro', category: 'Electronics', price: 250, image: 'https://images.unsplash.com/photo-1523275335684-37898b6baf30?w=400&q=80' },
  { id: 3, name: 'Minimalist Backpack', category: 'Accessories', price: 65, image: 'https://images.unsplash.com/photo-1553062407-98eeb64c6a62?w=400&q=80' },
  { id: 4, name: 'Mechanical Keyboard', category: 'Electronics', price: 95, image: 'https://images.unsplash.com/photo-1587829741301-dc798b83add3?w=400&q=80' },
  { id: 5, name: 'Classic Sunglasses', category: 'Accessories', price: 45, image: 'https://images.unsplash.com/photo-1572635196237-14b3f281503f?w=400&q=80' },
  { id: 6, name: 'Running Shoes', category: 'Footwear', price: 110, image: 'https://images.unsplash.com/photo-1542291026-7eec264c27ff?w=400&q=80' }
])

const searchQuery = ref('')
const selectedCategory = ref('All')
const cart = ref([])
const isCartOpen = ref(false)

// فلترة المنتجات حسب البحث والتصنيف
const filteredProducts = computed(() => {
  return products.value.filter(product => {
    const matchesSearch = product.name.toLowerCase().includes(searchQuery.value.toLowerCase())
    const matchesCategory = selectedCategory.value === 'All' || product.category === selectedCategory.value
    return matchesSearch && matchesCategory
  })
})

// إضافة منتج للسلة
const addToCart = (product) => {
  const existingItem = cart.value.find(item => item.id === product.id)
  if (existingItem) {
    existingItem.quantity++
  } else {
    cart.value.push({ ...product, quantity: 1 })
  }
}

// حذف منتج من السلة
const removeFromCart = (productId) => {
  cart.value = cart.value.filter(item => item.id !== productId)
}

// حساب إجمالي السعر
const totalPrice = computed(() => {
  return cart.value.reduce((total, item) => total + (item.price * item.quantity), 0