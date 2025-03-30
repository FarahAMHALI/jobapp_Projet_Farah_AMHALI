<template>
  <div class="tout">

      <h1>Available Jobs</h1>
      <div class="butt"><router-link to="/add"><button> Add Job </button></router-link></div>
      <div class="butt1">
          <div><button type="button" @click="toggleFilter">Filter</button></div>
          <div><FilterNav v-if="showFilter" @filter-jobs="filterJobs"/></div>
      </div>
      
      <div class="jobs">
          <div v-for="job in filteredJobs" :key="job.id" class="job">
              <router-link :to="'/jobs/' + job.id" class="link">{{ job.titre }}</router-link>
          </div>
      </div>

  </div>
</template>

<script>

import FilterNav from "./FilterNav.vue";

export default {
name: 'HomeView',
components : {FilterNav},
data(){
  return {
      showFilter : false ,
      jobs : [],
      filteredJobs: [],
  };
},
async mounted(){
  await this.fetchJobs();
},
methods:{
  async fetchJobs(){
      try{
          const response = await fetch("http://localhost:3000/jobs");
          this.jobs = await response.json();
          this.filteredJobs=this.jobs;
      }
      catch (error) {
          console.error("Error fetching jobs:", error);
      }
  },
  
  toggleFilter() {
      this.showFilter = !this.showFilter; // Toggle the visibility of the FilterNav component
      },
  filterJobs(text){
      const search = text.toLowerCase();
      this.filteredJobs = this.jobs.filter( (job) => job.titre.toLowerCase().includes(search) );
},
},

}
</script>



<style scoped>
.jobs {
  margin-top: 30px;
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  gap: 20px;
  margin-bottom: 60px;
}

.job {
  text-align: center;
  padding: 20px;
  border: 1px solid #e0e0e0;
  border-radius: 10px;
  background-color: #ffffff;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  transition: transform 0.2s, box-shadow 0.2s;
  cursor: pointer;
}

.job:hover {
  transform: translateY(-5px);
  box-shadow: 0 6px 12px rgba(0, 0, 0, 0.2);
}

.link {
  text-decoration: none;
  font-family: 'Roboto', Arial, sans-serif;
  font-size: 20px;
  font-weight: 600;
  color: #0056b3;
  transition: color 0.3s;
}

.link:hover {
  color: #003d80;
  text-decoration: underline;
}

h1 {
  font-family: 'Roboto', sans-serif;
  font-size: 32px;
  color: #222;
  text-align: center;
  margin-top: 30px;
  margin-bottom: 20px;
}

button {
  padding: 12px 25px;
  font-size: 16px;
  font-weight: bold;
  color: white;
  background-color: #007bff;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  transition: background-color 0.3s, transform 0.2s;
  width: 220px;
}

button:hover {
  background-color: #0056b3;
  transform: scale(1.05);
}

.butt {
 
  display: flex;
  justify-content: center;
  margin: 25px 0;
  margin-bottom: 60px;
}

.butt1 {
  
  display: flex;
  flex-direction: column;
  margin: 25px 0;
  margin-bottom: 60px;
  gap: 15px;
  align-items: center;
}

.tout {
  border: 2px solid #ccc;
  border-radius: 12px;
  padding: 25px;
  margin: 60px auto;
  max-width: 900px;
  background-color: #f8f9fa;
  box-shadow: 0 5px 10px rgba(0, 0, 0, 0.15);
}
</style>