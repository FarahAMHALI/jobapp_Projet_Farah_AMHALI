<template>
  <div v-if="job" class="tout">

      <h1>Job Details</h1>

      <div> 
          <h2>{{ job.titre }}</h2>
          <p>{{ job.description }}</p>
          <p>Salary: {{ job.salaire }}</p>
      </div>
      <div class="butt">
          <router-link :to="'/edit/' + job.id">  <button> Edit </button>  </router-link>
          <button type="button" @click="deletee"> Delete </button>
          <button type="button" @click="returnlist"> Return </button>
      </div>
      

  </div>
</template>

<script>
export default {
name: 'DetailsJob',
props:["id"],

data(){
  return {
      job : null
  };
},

async mounted(){
  await this.fetchJob();
},

methods : {
  async deletee(){
      try{
          const confirm = window.confirm("Êes vous sûrs de supprimer?");
          if(!confirm){
              return;
          }
          const response = await fetch(`http://localhost:3000/jobs/${this.id}`,{
          method: 'DELETE'});

          if (!response.ok) throw new Error(`HTTP error! Status: ${response.status}`);

          this.$router.push('/'); // return to home after adding job 
      }
      catch (error) {
          console.error("Error adding job:", error);
      }

  },
  returnlist(){
      this.$router.push('/');
  },
  async fetchJob(){
      try{
          const response = await fetch(`http://localhost:3000/jobs/${this.id}`);
          this.job = await response.json();
      }
      catch (error) {
          console.error("Error fetching jobs:", error);
      }
  }
}
}
</script>


<style scoped>


.tout {
  border: 1px solid #ccc; 
  border-radius: 8px; 
  padding: 25px; 
  margin: 40px auto; 
  max-width: 750px; 
  box-shadow: 0 3px 5px rgba(0, 0, 0, 0.15); 
  background-color: #ffffff;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
}

button {
  padding: 12px 25px;
  font-size: 15px;
  font-weight: 600;
  color: #ffffff;
  background-color: #007bff;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.3s ease;
  width: 140px;
}

button:hover {
  background-color: #0056b3;
}

.butt {
  display: flex;
  justify-content: space-around; 
  flex-direction: row;
  gap: 10px;
}



</style>
