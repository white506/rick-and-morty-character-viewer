<template>
  <article class="character-card">
    <div class="character-card__img-wrapper">
      <img
        :src="character.image"
        :alt="character.name"
        class="character-card__img"
      />
    </div>
    <div class="character-card__content">
      <div class="character-card__sections">
        <h2 class="character-card__name">{{ character.name }}</h2>
        <span class="character-card__status">
          <span
            :class="getStatusIconClass(character.status)"
            class="character-card__status-icon"
          ></span>
          {{ character.status }} - {{ character.species }}
        </span>
      </div>
      <div class="character-card__section">
        <span class="character-card__text-gray">Last known location:</span>
        <span class="character-card__location">{{
          character.location.name
        }}</span>
      </div>
      <div class="character-card__section">
        <span class="character-card__text-gray">First seen in:</span>
        <span class="character-card__episode">{{ firstEpisodeName }}</span>
      </div>
    </div>
  </article>
</template>

<script>
import axios from "axios";

export default {
  props: {
    character: {
      type: Object,
      required: true,
    },
  },
  data() {
    return {
      firstEpisodeName: "",
    };
  },
  mounted() {
    this.getFirstEpisodeName();
  },
  methods: {
    async getFirstEpisodeName() {
      try {
        const response = await axios.get(this.character.episode[0]);
        this.firstEpisodeName = response.data.name;
      } catch (error) {
        console.error("Error fetching episode data:", error);
      }
    },
    getStatusIconClass(status) {
      return {
        "character-card__status-icon": true,
        "character-card__status-icon--alive": status === "Alive",
        "character-card__status-icon--dead": status === "Dead",
        "character-card__status-icon--unknown": status === "unknown",
      };
    },
  },
};
</script>

<style scoped>
.character-card {
  width: 600px;
  height: 220px;
  display: flex;
  overflow: hidden;
  background: rgb(60, 62, 68);
  border-radius: 0.5rem;
  margin: 0.75rem;
  box-shadow: rgba(0, 0, 0, 0.1) 0px 4px 6px -1px,
    rgba(0, 0, 0, 0.06) 0px 2px 4px -1px;
}

.character-card__img-wrapper {
  flex: 0 0 40%;
}

.character-card__img {
  max-height: 100%;
}

.character-card__content {
  display: flex;
  flex-direction: column;
  padding: 10px;
  flex: 1;
  text-align: left;
}

.character-card__sections {
  margin-bottom: 10px;
  flex: 1 1 0%;
}
.character-card__section {
  margin-bottom: 10px;
  flex: 1 1 0%;
}

.character-card__name {
  font-size: 27px;
  margin: 0;
}

.character-card__status-icon {
  display: inline-block;
  width: 10px;
  height: 10px;
  border-radius: 50%;
}

.character-card__status-icon--alive {
  background-color: #55cc44;
}

.character-card__status-icon--dead {
  background-color: #d63d2e;
}

.character-card__status-icon--unknown {
  background-color: #cfda03;
}

.character-card__text-gray {
  color: #888;
}
.character-card__location,
.character-card__episode {
  font-size: 18px;
}

.character-card__section span {
  display: block;
}
</style>
