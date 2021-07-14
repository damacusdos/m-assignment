<template>
  <div class="record-container">
    <div class="search-container">
      <h1 class="page-title">Inventory Manager</h1>
      <div class="search-bar">
        <img src="../assets/images/search.png" alt="" class="search-icon">
        <input type="text" v-model="searchText" placeholder="Search for titles in inventory">
      </div>
    </div>
    <div class="table-header">
      <span class="record-icon head"></span>
      <span class="record-list-id head">ID</span>
      <span class="record-title head">Title Name</span>
      <span class="record-type head">Type</span>
      <span class="record-season head">Season</span>
      <span class="record-episode head">Episode</span>
      <span class="record-published head">Published</span>
      <span class="record-program head">Programmable</span>
    </div>
    <ul class="table-body">
      <li v-for="(info, index) of filteredInventory" :key="index" class="record-item"> 
        <div class="record-item-info">
            <div class="record-icon">
              <div v-if="info.seasons.length > 0" @click="openSeasonList(index)">
                <div v-if="toggleList[index].clicked === false" class="toggle-icon collapse"></div>
                <div v-else class="toggle-icon expand"></div>
              </div>
              <div v-else class="expand-icon"></div>
            </div>
            <span class="record-list-id">{{ info.title_id }}</span>
            <span class="record-title">{{ info.title_name }}</span>
            <span class="record-type">{{ info.content_type }}</span>
            <div class="record-season">
              <span v-if="info.content_type === 'Movie'">--</span>
              <span v-else class="record-season">{{ info.seasons.length }}</span>
            </div>
            <div class="record-episode">
              <span v-if="info.content_type === 'Movie'">--</span>
              <span v-else>{{ info.episode_count }}</span>
            </div>
            <span class="record-published">{{ transferTime(info.publish_timestamp) }}</span>
            <div class="record-program">
              <span v-if="info.content_type === 'Movie'">Single Movie</span>
              <span v-else>All Seasons</span>
            </div>
          </div>     
          <ul v-if="info.seasons.length > 0 && showSeasonList && index === clickedSeries" class="record-season-container">
            <li v-for="(season, i) in info.seasons" :key="i" class="season-item">
              <div class="season-list">
                <div class="record-icon" @click="openEpisodeList(i)">
                  <div v-if="!showEpisodeList" class="toggle-icon plus" />
                  <div v-else class="toggle-icon minus"  />
                </div>
                <span class="record-id">{{ season.season_id }}</span>
                <span class="record-season-title">{{ season.season_name }}</span>
                <span class="record-type">Season</span>
                <span class="record-season">S{{ season.season_number }}</span>
                <span class="record-episode">{{ season.episodes.length }}</span>
                <span class="record-published">{{ transferTime(season.publish_timestamp) }}</span>
                <span class="record-program">All Episodes</span>
              </div>
              <ul v-if="season.episodes.length > 0 && showEpisodeList && i === clickedSeason" class="record-episode-container">
                <li v-for="(episode, key) of season.episodes" :key="key" class="episode-item">
                  <div class="episode-list">
                    <span class="record-id">{{ episode.episode_id }}</span>
                    <span class="record-episode-title">{{ episode.episode_name }}</span>
                    <span class="record-type">Episode</span>
                    <span class="record-season">--</span>
                    <span class="record-episode">EP{{ episode.episode_number }}</span>
                    <span class="record-published">{{ transferTime(episode.publish_timestamp) }}</span>
                    <span class="record-program">Per Episode</span>
                  </div>
                </li>
            </ul>
          </li>
        </ul>
      </li>
    </ul>
  </div>
</template>

<script>
import data from '@/assets/data.json'

export default {
  data() {
    return {
      inventory: data,
      searchText: '',
      showSeasonList: false,
      showEpisodeList: false,
      clickedSeries: '',
      clickedSeason: '',
      clicked: false,
      toggleSeason: 'Collapse.png',
      toggleEpisode: 'Plus.png',   
    }
  },
  computed: {
    filteredInventory() {
      let filtered;
      const input = this.searchText.toLowerCase()
      if(this.searchText === '') {
        filtered = this.inventory
      } else {
        filtered = this.inventory.filter((item) => item.title_name.toLowerCase().includes(input));
      }
      return filtered
    },
    toggleList() {
      const checks = [];
      this.inventory.forEach( () => checks.push({clicked: false}));
      return checks;
    }
  },
  watch: {
    showSeasonList() {
      this.toggleSeason = this.toggleList[this.clickedSeries] === true ? 'Expand.png' : 'Collapse.png';
      return this.togglesSeason;
    },
    showEpisodeList() {
      this.toggleEpisode = this.showEpisodeList ? 'Minus.png' : 'Plus.png';
      return this.toggleEpisode;
    }
  },
  mounted() {
    console.log(this.inventory);
    console.log(this.toggleList);
  },
  methods: {
    openSeasonList(index) {
      this.clickedSeries = index;

      if(this.toggleList[index].clicked === true) {
        this.toggleList[index].clicked = false;
        this.showSeasonList = false;
      } else {
        this.toggleList.forEach( list => list.clicked = false);
        this.toggleList[index].clicked = true;
        this.showSeasonList = true
      }
    },
    openEpisodeList(index) {
      this.clickedSeason = index;
      this.showEpisodeList = !this.showEpisodeList;
    },
    transferTime(t) {
      const date =  new Date(t);
      const year = date.getFullYear();
      const month = date.toLocaleString('default', { month: 'short' });
      const day = date.getDate();
      return `${month} ${day}, ${year}`;
    }
  }
}
</script>

<style scoped>
.search-container {
  padding: 16px 0;
}
.page-title {
  font-size: 16px;
}
.search-bar {
  border: 1px solid #B2B2B2;
  border-radius: 4px;
  width: 419px;
  height: 40px;
  display: flex;
  align-items: center;
  padding: 0 .5rem;
  background-color:rgba(32, 32, 32, 0.05);
}
input {
  border: none;
  height: 90%;
  width: 80%;
  background-color: transparent;
}
input:focus {
  border: none;
  outline: none;
}
.record-container {
  padding: 16px;
}
.table-header {
  border: 1px solid #ccc;
  height: 48px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  border-radius: 4px 4px 0 0;
  background-color:rgba(32, 32, 32, 0.05);
}
.table-body {
  list-style-type: none;
  margin-top: 0;
  padding-left: 0;
  border-right: 1px solid #ccc;
  border-left: 1px solid #ccc;
}
.head {
  font-weight: 500;
}
.record-item {
  /* display: flex;
  align-items: center;
  justify-content: space-between;
  height: 48px; */
  border-bottom: 1px solid #ccc;
}
.record-item-detail {
  list-style-type: none;
  margin-top: 0;
  padding: 0;
  /* width: 100%; */
}
.record-item-info {
  width: 100%;
  display: flex;
  justify-content: space-between;
  align-items: center;
  height: 48px;
}
.record-icon {
  width: 20px;
  margin-left: 16px;
}
.record-id {
  width: 60px;
  margin-left: 16px;
  font-size: 12px;
}
.record-list-id {
  width: 60px;
  font-size: 12px;
}
.record-title {
  width: 400px;
  font-size: 12px;
  font-weight: 500;
}
.record-season-title {
  width: 350px;
  font-size: 12px;
  font-weight: 500;
}
.record-episode-title {
  width: 300px;
  font-size: 14px;
  font-weight: 500;
}
.record-type {
  width: 60px;
  font-size: 12px;
}
.record-season {
  width: 60px;
  font-size: 12px;
}
.record-episode {
  width: 60px;
  font-size: 12px;
}
.record-published {
  width: 250px;
  font-size: 12px;
}
.record-program {
  width: 100px;
  margin-right: 16px;
  font-size: 12px;
}
.record-season-container {
  border-top: 1px solid #ccc;
  list-style-type: none;
  padding-left: 0;
}
.season-item {
  border-bottom: 1px solid #ccc;
}
.season-list {
  height: 48px;
  display: flex;
  align-items: center;
  justify-content: space-evenly;
  margin-left: 50px;
}
.record-episode-container {
  list-style-type: none;
  padding-left: 0;
}
.episode-item {
  border-top: 1px solid #ccc;
}
.episode-list {
  height: 48px;
  display: flex;
  align-items: center;
  justify-content: space-evenly;
  margin-left: 125px;
}
.expand-icon {
  background-size: cover;
  width: 12px;
  height: 12px;
}
.toggle-icon {
  background-size: cover;
  width: 12px;
  height: 12px;
}
.plus {
  background-image: url('../assets/images/Plus.png');
}
.minus {
  background-image: url('../assets/images/Minus.png');
}
.expand {
  background-image: url('../assets/images/Expand.png')
}
.collapse {
  background-image: url('../assets/images/Collapse.png');
}
</style>
