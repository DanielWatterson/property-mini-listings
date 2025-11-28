<template>
  <div class="app">
    <!-- Header -->
    <header class="header">
      <div class="nav-bar">
        <div class="logo">🏡 Cozy Homes Co.</div>
        <h1 class="title">Property Listings</h1>
        <div class="wishlist">
          ❤️ Saved: <span>{{ wishlist.length }}</span>
        </div>
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

      <!-- Search & Sort -->
      <div class="search-sort">
        <input type="text" v-model="searchQuery" placeholder="Search by title or location..." />
        <button @click="toggleSort">Sort: {{ sortAsc ? 'Low → High' : 'High → Low' }}</button>
      </div>
    </header>

    <!-- Property Grid -->
    <main>
      <div class="property-grid">
        <PropertyCard
          v-for="property in sortedAndFilteredProperties"
          :key="property.title"
          :property="property"
          @add-to-wishlist="toggleSave"
        />
      </div>
    </main>
  </div>
</template>

<script>
import PropertyCard from './components/PropertyCard.vue'

export default {
  name: 'App',
  components: { PropertyCard },

  data() {
    return {
      showAvailableOnly: false,
      wishlist: [],
      searchQuery: '',
      loading: true,
      sortAsc: true, // true = low → high, false = high → low
      properties: [
        {
          title: 'Modern 3-Bedroom Apartment',
          agent: 'Listed by: RealtyPro SA',
          price: 85000,
          type: 'Apartment',
          location: 'Cape Town',
          available: true,
          image: 'https://i.postimg.cc/zv8vB7jF/3-Bedroom-Family-Apartment.jpg',
        },
        {
          title: 'Family Home with Garden',
          agent: 'Listed by: HomeSmart',
          price: 1200000,
          type: 'House',
          location: 'False Bay  ',
          available: false,
          image: 'https://i.postimg.cc/wMgMv2Vw/Family-Home-with-Garden.jpg',
        },
        {
          title: 'Camps Bay Home Villa',
          agent: 'Listed by: HouseLife SA',
          price: 200000000,
          type: 'Home Villa',
          location: 'Camps Bay',
          available: true,
          image: 'https://i.postimg.cc/brprJggf/Camps-Bay-Home-Villa.jpg',
        },
        {
          title: 'Art Deco Studio Apartment',
          agent: 'Listed by: HomeSmart',
          price: 1200000,
          type: 'Apartment',
          location: 'Johannesburg',
          available: true,
          image: 'https://i.postimg.cc/Bb4b6pg7/Art-Deco-Studio-Apartment.jpg',
        },
        {
          title: 'Luxury Penthouse Suite',
          agent: 'Listed by: Skyline Estates',
          price: 2100000,
          type: 'Penthouse',
          location: 'Johannesburg, Sandton',
          available: true,
          image: 'https://i.postimg.cc/8cNc54H0/Luxury-Penthouse-Suite.jpg',
        },
      ],
    }
  },

  computed: {
    // Filter by availability and search query
    filteredProperties() {
      return this.properties.filter((p) => {
        const matchesSearch =
          p.title.toLowerCase().includes(this.searchQuery.toLowerCase()) ||
          p.location.toLowerCase().includes(this.searchQuery.toLowerCase())
        return matchesSearch && (this.showAvailableOnly ? p.available : true)
      })
    },
    // Apply sorting after filtering
    sortedAndFilteredProperties() {
      return [...this.filteredProperties].sort((a, b) =>
        this.sortAsc ? a.price - b.price : b.price - a.price,
      )
    },
  },

  methods: {
    toggleSave(property) {
      const index = this.wishlist.findIndex((item) => item.title === property.title)
      if (index === -1) this.wishlist.push(property)
      else this.wishlist.splice(index, 1)
    },
    toggleSort() {
      this.sortAsc = !this.sortAsc
    },
  },
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

html,
body {
  width: 100%;
  font-family: 'Poppins', sans-serif;
  background: #f5f7fa;
}

.app {
  max-width: 1300px;
  margin: auto;
  padding: 20px;
}

/* Header Section --------------- */
.header {
  background: linear-gradient(90deg, #4b79a1, #283e51);
  padding: 20px;
  border-radius: 12px;
  color: white;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);
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
/* --------------- */

/* Filters SECTION --------------- */
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
/*  --------------- */

/* Search & Sort Section --------------- */
.search-sort {
  margin-top: 15px;
  display: flex;
  gap: 12px;
  justify-content: center;
  flex-wrap: wrap;
}

.search-sort input {
  padding: 8px 12px;
  border-radius: 20px;
  border: none;
  width: 250px;
  max-width: 100%;
}

.search-sort button {
  padding: 8px 18px;
  border-radius: 20px;
  border: none;
  background: #34495e;
  color: white;
  cursor: pointer;
  font-weight: 600;
}
/* --------------- */

/* Property Grid Section --------------- */
.property-grid {
  display: grid;
  gap: 25px;
  grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
}
/* Property Grid Section --------------- */

/* Responsiveness Section --------------- */
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
/* --------------- */

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

  .filters button,
  .search-sort button {
    padding: 6px 12px;
    font-size: 0.85rem;
  }

  .search-sort input {
    font-size: 0.85rem;
  }
}
</style>
