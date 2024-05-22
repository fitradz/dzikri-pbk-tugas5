<template>
  <q-layout view="lHh Lpr lFf" class="bg-white">
    <!-- Navbar -->
    <q-header elevated class="bg-gradient">
      <q-toolbar>
        <q-btn
          flat
          dense
          round
          @click="toggleLeftDrawer"
          aria-label="Menu"
          icon="menu"
        />
        <q-toolbar-title class="text-white">
          Tokopedia
        </q-toolbar-title>
        <q-input
          filled
          dense
          debounce="300"
          placeholder="Cari di Tokopedia"
          v-model="search"
          class="q-ml-md"
          rounded
        >
          <template v-slot:append>
            <q-icon name="search" />
          </template>
        </q-input>
      </q-toolbar>
    </q-header>

    <!-- Sidebar -->
    <q-drawer
      v-model="leftDrawerOpen"
      show-if-above
      bordered
      class="bg-grey-2"
    >
      <q-list>
        <q-item-label header>Essential Links</q-item-label>
        <q-item>
          <q-item-section>
            Home
          </q-item-section>
        </q-item>
        <q-item>
          <q-item-section>
            Categories
          </q-item-section>
        </q-item>
        <q-item>
          <q-item-section>
            Offers
          </q-item-section>
        </q-item>
        <!-- Add more list items here -->
      </q-list>
    </q-drawer>

    <!-- Main Content -->
    <q-page-container>
      <q-page padding>
        <!-- Carousel -->
        <div class="flex flex-center q-mb-md">
          <q-carousel v-model="slide" animated infinite transition-prev="slide-right" transition-next="slide-left">
            <q-carousel-slide v-for="(slide, index) in slides" :key="index">
              <q-img :src="slide" class="full-width" />
            </q-carousel-slide>
          </q-carousel>
        </div>

        <!-- Cards -->
        <div class="row justify-center q-col-gutter-md q-mt-md">
          <q-card
            v-for="(item, index) in items"
            :key="index"
            class="col-xs-12 col-sm-6 col-md-4 q-mb-md"
            @click="showDetails(item)"
            hoverable
            transition="scale"
          >
            <q-img :src="item.image" class="card-image">
              <q-badge color="red" floating>{{ item.discount }}</q-badge>
            </q-img>
            <q-card-section>
              <div class="text-subtitle1">{{ item.name }}</div>
              <div class="text-caption text-grey">{{ item.description }}</div>
              <div class="text-h6 text-primary q-mt-sm">{{ item.price }}</div>
              <q-btn
                flat
                round
                icon="shopping_cart"
                @click.stop="addToCart(item)"
                class="text-primary"
                aria-label="Add to cart"
                v-tooltip.bottom="'Add to cart'"
              />
            </q-card-section>
          </q-card>
        </div>

        <!-- Product Details Modal -->
        <q-dialog v-model="detailsDialog">
          <q-card class="details-card">
            <q-card-section>
              <q-img :src="selectedItem?.image" class="details-image" />
              <div class="text-h6">{{ selectedItem?.name }}</div>
              <div>{{ selectedItem?.description }}</div>
              <div class="text-h5 text-primary">{{ selectedItem?.price }}</div>
            </q-card-section>
            <q-card-actions align="right">
              <q-btn flat label="Close" color="primary" @click="closeDetailsDialog" />
            </q-card-actions>
          </q-card>
        </q-dialog>
      </q-page>
    </q-page-container>

    <!-- Footer -->
    <q-footer class="bg-gradient text-white q-pt-md q-pb-md">
      <div class="text-center">
        &copy; 2024 Tokopedia. All rights reserved.
      </div>
    </q-footer>
  </q-layout>
</template>

<script>
import { ref } from 'vue'
import { Quasar, QLayout, QHeader, QToolbar, QBtn, QToolbarTitle, QInput, QDrawer, QList, QItem, QItemSection, QItemLabel, QPageContainer, QPage, QCarousel, QCarouselSlide, QImg, QBadge, QCard, QCardSection, QFooter, QDialog, QCardActions, QTooltip } from 'quasar'

export default {
  name: 'MyLayout',
  components: {
    QLayout, QHeader, QToolbar, QBtn, QToolbarTitle, QInput, QDrawer, QList, QItem, QItemSection, QItemLabel, QPageContainer, QPage, QCarousel, QCarouselSlide, QImg, QBadge, QCard, QCardSection, QFooter, QDialog, QCardActions, QTooltip
  },
  setup () {
    const leftDrawerOpen = ref(false)
    const search = ref('')
    const slide = ref(0)
    const detailsDialog = ref(false)
    const selectedItem = ref(null)
    const slides = [
      'https://via.placeholder.com/800x400?text=Slide+1',
      'https://via.placeholder.com/800x400?text=Slide+2',
      'https://via.placeholder.com/800x400?text=Slide+3'
    ]
    const items = [
      { name: 'Blok Mesin', description: 'Untuk Zx - Buy 3 Get 1', price: 'Rp3.500.999', discount: '36%', image: 'https://s3-id-jkt-1.kilatstorage.id/fastnlow/2018/09/Sebenarnya-Apa-Fungsi-Dari-Cylinder-Block-.jpg' },
      { name: 'Piston Motor', description: '', price: 'Rp350.700', discount: '23%', image: 'https://fthmb.tqn.com/UOcBsTY7ayyTFP2mTc3_1iAZn-Q=/3157x2501/filters:fill(auto,1)/engine-internals-56cb760a3df78cfb379d2755.jpg' }
    ]

    function toggleLeftDrawer () {
      leftDrawerOpen.value = !leftDrawerOpen.value
    }

    function showDetails(item) {
      selectedItem.value = item
      detailsDialog.value = true
    }

    function addToCart(item) {
      console.log(`Added ${item.name} to cart`)
      // Tambahkan logika untuk menambahkan item ke keranjang di sini
    }

    function closeDetailsDialog() {
      detailsDialog.value = false
    }

    return {
      leftDrawerOpen,
      toggleLeftDrawer,
      search,
      slide,
      slides,
      items,
      detailsDialog,
      selectedItem,
      showDetails,
      addToCart,
      closeDetailsDialog
    }
  }
}
</script>

<style>
.card-image {
  height: 200px;
  width: 100%;
  object-fit: cover;
}
.flex-center {
  display: flex;
  justify-content: center;
  align-items: center;
}
.q-card:hover {
  box-shadow: 0 8px 16px rgba(0, 0, 0, 0.2);
  transition: box-shadow 0.3s ease;
}
</style>
<style>
.card-image {
  height: 200px;
  width: 100%;
  object-fit: cover;
}
.details-image {
  height: 300px;
  width: 100%;
  object-fit: cover;
}
.flex-center {
  display: flex;
  justify-content: center;
  align-items: center;
}
.q-card:hover {
  box-shadow: 0 8px 16px rgba(0, 0, 0, 0.2);
  transition: box-shadow 0.3s ease;
}
.bg-gradient {
  background: linear-gradient(to right, #ff7e5f, #feb47b);
}
.details-card {
  max-width: 600px;
  margin: auto;
}
</style>
