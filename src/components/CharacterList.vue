<template>
  <div class="character-list">
    <div class="character-list__filters">
      <label class="character-list__label" for="name">Name:</label>
      <input
        type="text"
        v-model="nameFilter"
        id="name"
        class="character-list__input"
      />
      <label class="character-list__label" for="status">Status:</label>
      <select v-model="statusFilter" id="status" class="character-list__select">
        <option value="">All</option>
        <option value="alive">Alive</option>
        <option value="dead">Dead</option>
        <option value="unknown">Unknown</option>
      </select>
      <button @click="applyFilters" class="character-list__button">
        Apply
      </button>
    </div>
    <div class="character-list__pagination">
      <button
        @click="loadPreviousPage"
        :disabled="!info.prev"
        class="character-list__pagination-button"
      >
        Previous
      </button>
      <button
        v-for="pageNumber in visiblePageNumbers"
        :key="pageNumber"
        :class="{
          'character-list__pagination-button--active':
            pageNumber === currentPage,
        }"
        @click="goToPage(pageNumber)"
        class="character-list__pagination-button"
      >
        {{ pageNumber }}
      </button>
      <button
        @click="loadNextPage"
        :disabled="!info.next"
        class="character-list__pagination-button"
      >
        Next
      </button>
    </div>
    <div class="character-list__cards">
      <CharacterCard
        v-for="character in characters"
        :key="character.id"
        :character="character"
      />
    </div>
    <div class="character-list__pagination">
      <button
        @click="loadPreviousPage"
        :disabled="!info.prev"
        class="character-list__pagination-button"
      >
        Previous
      </button>
      <button
        v-for="pageNumber in visiblePageNumbers"
        :key="pageNumber"
        :class="{
          'character-list__pagination-button--active':
            pageNumber === currentPage,
        }"
        @click="goToPage(pageNumber)"
        class="character-list__pagination-button"
      >
        {{ pageNumber }}
      </button>
      <button
        @click="loadNextPage"
        :disabled="!info.next"
        class="character-list__pagination-button"
      >
        Next
      </button>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import CharacterCard from "./CharacterCard.vue";

export default {
  components: {
    CharacterCard,
  },
  data() {
    return {
      characters: [],
      info: {},
      currentPage: 1,
      nameFilter: "",
      statusFilter: "",
    };
  },
  computed: {
    visiblePageNumbers() {
      const maxVisiblePages = 10;
      const lastPage = this.info.pages;
      let startPage = Math.max(
        this.currentPage - Math.floor(maxVisiblePages / 2),
        1
      );
      let endPage = Math.min(startPage + maxVisiblePages - 1, lastPage);
      if (endPage - startPage + 1 < maxVisiblePages) {
        startPage = Math.max(endPage - maxVisiblePages + 1, 1);
      }
      return Array.from(
        { length: endPage - startPage + 1 },
        (_, i) => startPage + i
      );
    },
  },
  async mounted() {
    await this.loadPage("https://rickandmortyapi.com/api/character");
  },
  methods: {
    async loadPage(url) {
      try {
        const response = await axios.get(url);
        this.characters = response.data.results;
        this.info = response.data.info;
      } catch (error) {
        console.error("Ошибка при получении данных о персонажах:", error);
      }
    },
    async loadPreviousPage() {
      if (this.info.prev) {
        await this.loadPage(this.info.prev);
        this.currentPage--;
      }
    },
    async loadNextPage() {
      if (this.info.next) {
        await this.loadPage(this.info.next);
        this.currentPage++;
      }
    },
    async goToPage(pageNumber) {
      const url = `https://rickandmortyapi.com/api/character?page=${pageNumber}`;
      await this.loadPage(url);
      this.currentPage = pageNumber;
    },
    async applyFilters() {
      let url = "https://rickandmortyapi.com/api/character";
      if (this.nameFilter || this.statusFilter) {
        url += `?name=${this.nameFilter}&status=${this.statusFilter}`;
      }
      await this.loadPage(url);
      this.currentPage = 1;
    },
  },
};
</script>

<style>
.character-list {
  padding: 20px;
  text-align: center;
}

.character-list__filters {
  margin-bottom: 20px;
  display: flex;
  justify-content: center;
  align-items: center;
}

.character-list__label {
  margin-right: 10px;
  color: #ffffffde;
}

.character-list__input,
.character-list__select {
  margin-right: 10px;
  padding: 8px;
  border-radius: 4px;
  border: 1px solid #ffffffde;
  background-color: transparent;
  color: #ffffffde;
}

.character-list__input:focus,
.character-list__select:focus {
  outline: none;
  border-color: #ff9800;
}

.character-list__button {
  padding: 8px 16px;
  border-radius: 4px;
  border: 1px solid #ff9800;
  background-color: #ff9800;
  color: #ffffff;
  cursor: pointer;
  transition: background-color 0.3s, color 0.3s, border-color 0.3s;
}

.character-list__button:hover {
  background-color: #ff9800;
  color: #ffffff;
  border-color: #ff9800;
}

.character-list__pagination {
  margin-top: 20px;
  margin-bottom: 20px;
  display: flex;
  justify-content: center;
  align-items: center;
}

.character-list__pagination-button {
  padding: 8px 16px;
  border-radius: 4px;
  border: 1px solid #ffffff;
  background-color: transparent;
  color: #ffffff;
  cursor: pointer;
  margin: 0 5px;
  transition: color 0.3s, border-color 0.3s;
}

.character-list__pagination-button:hover {
  color: #ff9800;
  border-color: #ff9800;
}

.character-list__pagination-button--active {
  font-weight: bold;
  color: #ff9800;
}

.character-list__pagination-label {
  margin: 0 5px;
  color: #ffffffde;
}

.character-list__cards {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  align-items: center;
  padding: 4.5rem 0px;
  background: rgb(39, 43, 51);
  min-height: calc(-60px + 50vh);
}

select option {
  color: white;
  background-color: #242424;
}
</style>
