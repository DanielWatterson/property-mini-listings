<template>
  <div class="app">
    <!-- Header -->
    <header class="header">
      <div class="nav-bar">
        <div class="logo">🏡 HomeFinder</div>
        <h1 class="title">Property Listings</h1>
        <div class="wishlist">❤️ Saved: <span>{{ wishlist.length }}</span></div>
      </div>

      <!-- Filters row -->
      <div class="filters">
        <button @click="showAvailableOnly = false" :class="{ active: !showAvailableOnly }">
          All Properties
        </button>
        <button @click="showAvailableOnly = true" :class="{ active: showAvailableOnly }">
          Available Only
        </button>
      </div>
    </header>

    <!-- Property Grid -->
    <main>
      <div class="property-grid">
        <PropertyCard
          v-for="property in filteredProperties"
          :key="property.title"
          :property="property"
          @add-to-wishlist="toggleSave"
        />
      </div>
    </main>
  </div>
</template>

<script>
import PropertyCard from "./components/PropertyCard.vue";

export default {
  name: "App",
  components: { PropertyCard },

  data() {
    return {
      showAvailableOnly: false,
      wishlist: [],
      properties: [
        {
          title: "Modern 3-Bedroom Apartment",
          agent: "Listed by: RealtyPro SA",
          price: "R850,000",
          type: "Apartment",
          location: "Cape Town",
          available: true,
          image: "https://i.postimg.cc/NjgWTbTt/faw-chefs-knives.jpg",
        },
        {
          title: "Family Home with Garden",
          agent: "Listed by: HomeSmart",
          price: "R1,200,000",
          type: "House",
          location: "Johannesburg",
          available: false,
          image: "https://i.postimg.cc/dVDn9vTP/istockphoto-1127973172.jpg",
        },
        {
          title: "Luxury Penthouse Suite",
          agent: "Listed by: Skyline Estates",
          price: "R2,100,000",
          type: "Penthouse",
          location: "Durban",
          available: true,
          image: "https://i.postimg.cc/4xnw1ZhR/2548301.jpg",
        },
      ],
    };
  },

  computed: {
    filteredProperties() {
      return this.showAvailableOnly
        ? this.properties.filter((p) => p.available)
        : this.properties;
    },
  },

  methods: {
    toggleSave(property) {
      const index = this.wishlist.findIndex((item) => item.title === property.title);
      if (index === -1) this.wishlist.push(property);
      else this.wishlist.splice(index, 1);
    },
  },
};
</script>

<style>
/* Global reset */
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

html, body {
  width: 100%;
  font-family: "Poppins", sans-serif;
  background: #f5f7fa;
}

.app {
  max-width: 1300px;
  margin: auto;
  padding: 20px;
}

/* Header */
.header {
  background: linear-gradient(90deg, #4b79a1, #283e51);
  padding: 20px;
  border-radius: 12px;
  color: white;
  box-shadow: 0 4px 12px rgba(0,0,0,0.15);
  margin-bottom: 35px;
}

.nav-bar {
  display: flex;
  justify-content: space-between;
  align-items: center;
  flex-wrap: wrap;
}

.logo {
  font-size: 1.7rem;
  font-weight: 700;
}

.title {
  font-size: 1.5rem;
  text-align: center;
  flex: 1;
  margin: 10px 0;
  font-weight: 600;
}

.wishlist {
  font-weight: bold;
  font-size: 1.2rem;
}

/* Filters */
.filters {
  margin-top: 15px;
  display: flex;
  gap: 12px;
  justify-content: center;
  flex-wrap: wrap;
}

.filters button {
  padding: 8px 18px;
  border-radius: 20px;
  border: none;
  cursor: pointer;
  font-weight: 600;
  background: white;
  color: #34495e;
  transition: 0.25s ease;
}

.filters button.active {
  background: #1b263b;
  color: white;
}

.filters button:hover {
  transform: translateY(-2px);
}

/* Property Grid */
.property-grid {
  display: grid;
  gap: 25px;
  grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
}

/* Responsive */
@media (max-width: 768px) {
  .nav-bar {
    flex-direction: column;
    text-align: center;
  }

  .title {
    margin: 8px 0;
  }

  .property-grid {
    grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
  }
}

@media (max-width: 480px) {
  .title {
    font-size: 1.2rem;
  }

  .logo {
    font-size: 1.3rem;
  }

  .wishlist {
    font-size: 1rem;
  }

  .property-grid {
    grid-template-columns: 1fr;
    gap: 15px;
  }

  .filters button {
    padding: 6px 12px;
    font-size: 0.85rem;
  }
}
</style>
