<template>
  <div class="course-card">

    <!-- IMAGE -->
    <div class="image-container">
      <img :src="course.image" :alt="course.title" class="course-image" />
      <div v-if="!course.available" class="sold-overlay">SOLD OUT</div>
    </div>

    <!-- TEXT -->
    <h2 class="title">{{ course.title }}</h2>
    <p class="chef">By {{ course.chef }}</p>
    <p class="price">{{ course.price }}</p>
    <p class="level" :class="course.level.toLowerCase()">
      {{ course.level }}
    </p>

    <!-- BUTTONS -->
    <div class="button-row">
      <button 
        :disabled="!course.available" 
        @click="bookCourse(course.title)" 
        class="book-btn"
      >
        {{ course.available ? "Book" : "Unavailable" }}
      </button>

      <button 
        @click="$emit('add-to-wishlist', course)" 
        class="save-btn"
      >
        Save
      </button>
    </div>

  </div>
</template>

<script>
export default {
  name: "CourseCard",
  props: {
    course: Object,
  },
  methods: {
    bookCourse(title) {
      alert(`You booked: ${title}`);
    },
  },
};
</script>

<style scoped>
/* This container has the entire card */
.course-card {
  background: #ffffff;
  border-radius: 10px;
  padding: 16px;
  display: flex;
  flex-direction: column;
  box-shadow: 0 4px 14px rgba(0,0,0,0.1);
  transition: transform .15s ease;
}

.course-card:hover {
  transform: translateY(-3px);
}

/* This container has the images within them. */
.image-container {
  width: 100%;
  aspect-ratio: 4/3;
  border-radius: 10px;
  overflow: hidden;
  position: relative;
}

.course-image {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.sold-overlay {
  position: absolute;
  inset: 0;
  background: rgba(255, 70, 70, 0.7);
  color: white;
  display: flex;
  justify-content: center;
  align-items: center;
  font-weight: 800;
  font-size: 1.3rem;
}

.title {
  margin: 12px 0 4px;
  font-size: 1.1rem;
  font-weight: 700;
}

.chef {
  color: #444;
  font-size: 0.9rem;
  margin-bottom: 4px;
}

.price {
  font-size: 1rem;
  font-weight: 600;
  color: #333;
  margin-bottom: 4px;
}

/* A level tag was given for extra flair, showing how advanced certain classes are. */
.level {
  font-size: 0.85rem;
  padding: 3px 8px;
  border-radius: 6px;
  display: inline-block;
  margin-bottom: 10px;
  font-weight: bold;
}

.beginner {
  background: #e1f6e8;
  color: #2a8a47;
}

.intermediate {
  background: #fff4d6;
  color: #c69200;
}

.advanced {
  background: #ffe4e4;
  color: #c23535;
}

/* BUTTONS styling here, keeps a flex design to the buttons */
.button-row {
  display: flex;
  gap: 10px;
  margin-top: auto;
}

.book-btn,
.save-btn {
  flex: 1;
  padding: 10px 0;
  border-radius: 6px;
  border: none;
  font-weight: 700;
  cursor: pointer;
  transition: 0.15s ease;
  color: white;
}

.book-btn {
  background: #ff6347;
}

.book-btn:hover:not(:disabled) {
  background: #ff4b2b;
}

.book-btn:disabled {
  background: #c9c9c9;
  cursor: not-allowed;
}

/* SAVE button to save course to wishlist */
.save-btn {
  background: #ffb36a;
}

.save-btn:hover {
  background: #ff9a3d;
}
</style>
