<template>
  <div class="anime-recommender">
    <h1 class="title">Anime Recommender</h1>
    <p class="description">Discover your next favorite anime with just one click!</p>

    <button @click="getRandomAnime" class="recommend-button" :disabled="loading">
      Get Random Anime
    </button>

    <!-- Enhanced Lightning Loading Animation -->
    <div v-if="loading" class="lightning-container">
      <div class="particle-container">
        <div v-for="n in 50" :key="n" class="particle"></div>
      </div>
      <div class="lightning-core"></div>
    </div>

    <div v-if="anime && !loading" class="anime-container">
      <!-- Main Info Section -->
      <section class="info-section main-info">
        <div class="anime-header">
          <img :src="anime.data.images.jpg.large_image_url" alt="Anime Cover" class="anime-image" />
          <div class="anime-titles">
            <h2>{{ anime.data.title }}</h2>
            <h3>{{ anime.data.title_japanese }}</h3>
            <h4>{{ anime.data.title_english }}</h4>
            <a :href="anime.data.url" target="_blank" class="mal-link">
              View on MyAnimeList
              <span class="arrow">→</span>
            </a>
          </div>
        </div>
      </section>

      <!-- Quick Stats Section -->
      <section class="info-section stats">
        <h3 class="section-title">Quick Stats</h3>
        <div class="stats-grid">
          <div class="stat-item">
            <span class="stat-label">Score</span>
            <span class="stat-value">{{ anime.data.score }}/10</span>
            <span class="stat-sub">by {{ anime.data.scored_by }} users</span>
          </div>
          <div class="stat-item">
            <span class="stat-label">Rank</span>
            <span class="stat-value">#{{ anime.data.rank }}</span>
          </div>
          <div class="stat-item">
            <span class="stat-label">Popularity</span>
            <span class="stat-value">#{{ anime.data.popularity }}</span>
          </div>
          <div class="stat-item">
            <span class="stat-label">Members</span>
            <span class="stat-value">{{ anime.data.members.toLocaleString() }}</span>
          </div>
        </div>
      </section>

      <!-- Synopsis Section -->
      <section class="info-section">
        <h3 class="section-title">Synopsis</h3>
        <p class="synopsis">{{ anime.data.synopsis }}</p>
        <div v-if="anime.data.background" class="background">
          <h4>Background</h4>
          <p>{{ anime.data.background }}</p>
        </div>
      </section>

      <!-- Details Section -->
      <section class="info-section">
        <h3 class="section-title">Details</h3>
        <div class="details-grid">
          <div class="detail-item">
            <span class="detail-label">Type</span>
            <span>{{ anime.data.type }}</span>
          </div>
          <div class="detail-item">
            <span class="detail-label">Episodes</span>
            <span>{{ anime.data.episodes }}</span>
          </div>
          <div class="detail-item">
            <span class="detail-label">Status</span>
            <span>{{ anime.data.status }}</span>
          </div>
          <div class="detail-item">
            <span class="detail-label">Aired</span>
            <span>{{ anime.data.aired.string }}</span>
          </div>
          <div class="detail-item">
            <span class="detail-label">Season</span>
            <span>{{ anime.data.season }} {{ anime.data.year }}</span>
          </div>
          <div class="detail-item">
            <span class="detail-label">Duration</span>
            <span>{{ anime.data.duration }}</span>
          </div>
          <div class="detail-item">
            <span class="detail-label">Rating</span>
            <span>{{ anime.data.rating }}</span>
          </div>
        </div>
      </section>

      <!-- Genres Section -->
      <section class="info-section">
        <h3 class="section-title">Genres</h3>
        <div class="tags-container">
          <span v-for="genre in anime.data.genres" :key="genre.mal_id" class="tag genre-tag">
            {{ genre.name }}
          </span>
        </div>
      </section>

      <!-- Themes Section -->
      <section class="info-section" v-if="anime.data.themes.length">
        <h3 class="section-title">Themes</h3>
        <div class="tags-container">
          <span v-for="theme in anime.data.themes" :key="theme.mal_id" class="tag theme-tag">
            {{ theme.name }}
          </span>
        </div>
      </section>

      <!-- Studios & Producers Section -->
      <section class="info-section">
        <h3 class="section-title">Production</h3>
        <div class="production-info">
          <div v-if="anime.data.studios.length">
            <h4>Studios</h4>
            <div class="tags-container">
              <span
                v-for="studio in anime.data.studios"
                :key="studio.mal_id"
                class="tag studio-tag"
              >
                {{ studio.name }}
              </span>
            </div>
          </div>
          <div v-if="anime.data.producers.length">
            <h4>Producers</h4>
            <div class="tags-container">
              <span
                v-for="producer in anime.data.producers"
                :key="producer.mal_id"
                class="tag producer-tag"
              >
                {{ producer.name }}
              </span>
            </div>
          </div>
        </div>
      </section>

      <!-- Broadcast Section -->
      <section class="info-section" v-if="anime.data.broadcast.string">
        <h3 class="section-title">Broadcast Information</h3>
        <div class="broadcast-info">
          <p>{{ anime.data.broadcast.string }}</p>
          <p>Timezone: {{ anime.data.broadcast.timezone }}</p>
        </div>
      </section>
    </div>
  </div>
</template>

<style scoped>
.anime-recommender {
  max-width: 1200px;
  margin: 0 auto;
  padding: 20px;
}

.title {
  color: #c77dff;
  font-size: 3.5em;
  text-align: center;
  margin-bottom: 20px;
  text-shadow: 0 0 10px rgba(199, 125, 255, 0.3);
}

.description {
  text-align: center;
  font-size: 1.2em;
  margin-bottom: 30px;
  color: #a0c4ff;
}

.recommend-button {
  display: block;
  margin: 0 auto;
  padding: 15px 30px;
  font-size: 1.2em;
  background: linear-gradient(45deg, #5a189a, #7b2cbf);
  color: #fff;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  transition: all 0.3s ease;
  box-shadow: 0 4px 15px rgba(90, 24, 154, 0.3);
}

.recommend-button:hover {
  transform: scale(1.05);
  box-shadow: 0 6px 20px rgba(90, 24, 154, 0.4);
}

.anime-container {
  margin-top: 30px;
}

.info-section {
  background: rgba(90, 24, 154, 0.1);
  border-radius: 15px;
  padding: 25px;
  margin-bottom: 20px;
  backdrop-filter: blur(10px);
  border: 1px solid rgba(199, 125, 255, 0.1);
}

.section-title {
  color: #c77dff;
  font-size: 1.8em;
  margin-bottom: 20px;
  border-bottom: 2px solid rgba(199, 125, 255, 0.3);
  padding-bottom: 10px;
}

.anime-header {
  display: flex;
  gap: 30px;
  align-items: flex-start;
}

.anime-image {
  width: 300px;
  border-radius: 10px;
  box-shadow: 0 5px 15px rgba(0, 0, 0, 0.3);
}

.mal-link {
  display: inline-block;
  margin-top: 15px;
  color: #c77dff;
  text-decoration: none;
  font-weight: bold;
  transition: all 0.3s ease;
}

.mal-link:hover {
  color: #a0c4ff;
}

.arrow {
  display: inline-block;
  transition: transform 0.3s ease;
}

.mal-link:hover .arrow {
  transform: translateX(5px);
}

.stats-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
  gap: 20px;
}

.stat-item {
  text-align: center;
  padding: 15px;
  background: rgba(90, 24, 154, 0.2);
  border-radius: 10px;
}

.stat-label {
  display: block;
  color: #a0c4ff;
  font-size: 0.9em;
  margin-bottom: 5px;
}

.stat-value {
  display: block;
  color: #c77dff;
  font-size: 1.8em;
  font-weight: bold;
}

.stat-sub {
  display: block;
  font-size: 0.8em;
  color: #a0c4ff;
  opacity: 0.8;
}

.details-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: 15px;
}

.detail-item {
  padding: 10px;
  background: rgba(90, 24, 154, 0.2);
  border-radius: 8px;
}

.detail-label {
  color: #a0c4ff;
  margin-right: 10px;
}

.tags-container {
  display: flex;
  flex-wrap: wrap;
  gap: 10px;
}

.tag {
  padding: 8px 15px;
  border-radius: 20px;
  font-size: 0.9em;
  transition: all 0.3s ease;
}

.genre-tag {
  background: linear-gradient(45deg, #5a189a, #7b2cbf);
}

.theme-tag {
  background: linear-gradient(45deg, #7b2cbf, #9d4edd);
}

.studio-tag {
  background: linear-gradient(45deg, #9d4edd, #c77dff);
}

.producer-tag {
  background: linear-gradient(45deg, #c77dff, #e0aaff);
}

/* Enhanced Lightning Animation */
.lightning-container {
  position: relative;
  height: 300px;
  display: flex;
  justify-content: center;
  align-items: center;
  margin: 30px 0;
  overflow: hidden;
}

.lightning-core {
  width: 4px;
  height: 200px;
  background: #c77dff;
  position: relative;
  animation: lightning-pulse 1.5s infinite;
  box-shadow:
    0 0 20px #c77dff,
    0 0 40px #c77dff,
    0 0 60px #c77dff;
}

.particle-container {
  position: absolute;
  width: 100%;
  height: 100%;
}

.particle {
  position: absolute;
  width: 2px;
  height: 2px;
  background: #c77dff;
  border-radius: 50%;
  animation: particle-animation 2s infinite;
}

@keyframes lightning-pulse {
  0%,
  100% {
    transform: scaleY(0.3);
    opacity: 0.3;
  }
  50% {
    transform: scaleY(1);
    opacity: 1;
  }
}

@keyframes particle-animation {
  0% {
    transform: translate(0, 0);
    opacity: 1;
  }
  100% {
    transform: translate(calc(random(100) * 1px - 50px), calc(random(100) * 1px - 50px));
    opacity: 0;
  }
}

/* Add responsive design */
@media (max-width: 768px) {
  .anime-header {
    flex-direction: column;
    align-items: center;
  }

  .anime-image {
    width: 100%;
    max-width: 300px;
  }

  .stats-grid {
    grid-template-columns: repeat(2, 1fr);
  }
}
</style>

<script>
import axios from 'axios'

export default {
  name: 'AnimeRecommender',
  data() {
    return {
      anime: null,
      loading: false,
      error: null,
    }
  },
  methods: {
    async getRandomAnime() {
      this.loading = true
      try {
        const response = await axios.get('http://localhost:8080/api/random-anime')
        this.anime = response.data
      } catch (error) {
        this.error = error.message
      } finally {
        setTimeout(() => {
          this.loading = false
        }, 1500) // Minimum loading time to show animation
      }
    },
    // Helper method to generate random particles
    mounted() {
      const particles = document.querySelectorAll('.particle')
      particles.forEach((particle) => {
        particle.style.left = `${Math.random() * 100}%`
        particle.style.top = `${Math.random() * 100}%`
        particle.style.animationDelay = `${Math.random() * 2}s`
      })
    },
  },
}
</script>
