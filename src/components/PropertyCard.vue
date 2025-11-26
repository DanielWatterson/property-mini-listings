<template>
  <div class="property-card">
    <div class="image-container">
      <img :src="property.image" :alt="property.title" class="property-image" />
      <div v-if="!property.available" class="sold-overlay">SOLD</div>
    </div>

    <h2 class="title">{{ property.title }}</h2>
    <p class="location">📍 {{ property.location }}</p>
    <p class="price">{{ property.price }}</p>
    <p class="type-tag">{{ property.type }}</p>

    <div class="button-row">
      <button :disabled="!property.available" @click="viewProperty(property.title)" class="view-btn">
        {{ property.available ? "View Property" : "Unavailable" }}
      </button>
      <button @click="$emit('add-to-wishlist', property)" class="save-btn">
        Save
      </button>
    </div>
  </div>
</template>

<script>
export default {
  name: "PropertyCard",
  props: { property: Object },
  methods: {
    viewProperty(title) {
      alert(`Opening listing: ${title}`);
    },
  },
};
</script>

<style scoped>
.property-card {
  background: #fff;
  border-radius: 14px;
  padding: 16px;
  display: flex;
  flex-direction: column;
  box-shadow: 0 4px 18px rgba(0,0,0,0.12);
  transition: transform .15s ease, box-shadow .15s ease;
}

.property-card:hover {
  transform: translateY(-4px);
  box-shadow: 0 6px 22px rgba(0,0,0,0.18);
}

.image-container {
  width: 100%;
  aspect-ratio: 4/3;
  border-radius: 12px;
  overflow: hidden;
  position: relative;
}

.property-image {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.sold-overlay {
  position: absolute;
  inset: 0;
  background: rgba(0,0,0,0.55);
  color: white;
  display: flex;
  justify-content: center;
  align-items: center;
  font-weight: 800;
  font-size: 1.4rem;
  letter-spacing: 2px;
}

.title {
  margin: 14px 0 4px;
  font-size: 1.2rem;
  font-weight: 700;
  color: #222;
}

.location {
  color: #555;
  font-size: 0.95rem;
  margin-bottom: 6px;
}

.price {
  font-size: 1.1rem;
  font-weight: 600;
  color: #1a3d7c;
  margin-bottom: 8px;
}

.type-tag {
  font-size: 0.85rem;
  padding: 4px 10px;
  background: #e9f1ff;
  color: #1a3d7c;
  border-radius: 6px;
  display: inline-block;
  font-weight: 600;
  margin-bottom: 12px;
}

.button-row {
  display: flex;
  gap: 10px;
  margin-top: auto;
}

.view-btn, .save-btn {
  flex: 1;
  padding: 11px 0;
  border-radius: 6px;
  border: none;
  font-weight: 700;
  cursor: pointer;
  transition: 0.15s ease;
  color: white;
}

.view-btn {
  background: #1a3d7c;
}

.view-btn:hover:not(:disabled) {
  background: #102a56;
}

.view-btn:disabled {
  background: #b0b0b0;
  cursor: not-allowed;
}

.save-btn {
  background: #ffb36a;
}

.save-btn:hover {
  background: #ff9a3d;
}

/* Responsive */
@media (max-width: 768px) {
  .title { font-size: 1.1rem; }
  .location, .price { font-size: 0.9rem; }
  .type-tag { font-size: 0.8rem; padding: 3px 8px; }
  .view-btn, .save-btn { padding: 9px 0; font-size: 0.95rem; }
}

@media (max-width: 480px) {
  .title { font-size: 1rem; }
  .location, .price { font-size: 0.85rem; }
  .type-tag { font-size: 0.75rem; padding: 2px 6px; }
  .view-btn, .save-btn { padding: 8px 0; font-size: 0.9rem; }
}
</style>
