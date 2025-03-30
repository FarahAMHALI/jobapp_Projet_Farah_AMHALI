<template>
    <div class="tout">
      <h1>Add a New Job</h1>
      <form @submit.prevent="addjob" class="jobs">
        <div class="job"><input v-model="job.titre" placeholder="Titre" required /></div>
        <div class="job"><input v-model="job.description" placeholder="Description" required /> </div>
        <div class="job"><input v-model="job.salaire" placeholder="Salaire" required /> </div>
        <div class="butt">
          <div ><button type="submit">Add Job</button></div>
          <div ><button type="button" @click="returnlist">Return </button></div>
        </div>
      </form>

    </div>
  </template>
  
  <script>
  export default {
    name: 'AddJob',
    data(){
        return {
            job : {titre:'', description:'', salaire:''},
        };
    },
    methods: {
        async addjob(){
            try{
                const response = await fetch("http://localhost:3000/jobs",{
                method: 'POST',
                headers: {'Content-Type': 'application/json'},
                body:JSON.stringify(this.job)}
                );

                if (!response.ok) throw new Error(`HTTP error! Status: ${response.status}`);

                this.$router.push('/'); 
            }
            catch (error) {
                console.error("Error adding job:", error);
            }
        },
        returnlist(){
            this.$router.push('/');
        }
    }
  }
  </script>

<style scoped>

.jobs {
  margin-top: 20px;
  display: flex;
  flex-direction: column;
  gap: 20px;
  align-items: center;
  margin-bottom: 50px;
}

.job {
  text-align: center;
  padding: 10px;
  width: 60%;
  background-color: #f0f0f0;
  border: 1px solid #ccc;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

h1 {
  font-family: 'Arial', sans-serif;
  font-size: 32px;
  color: #444;
  text-align: center;
  margin-top: 20px;
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
  width: 160px;
}

button:hover {
  background-color: #0056b3;
  transform: scale(1.05);
}

.tout {
  border: 2px solid #ccc;
  border-radius: 12px;
  padding: 30px;
  margin: 50px auto;
  max-width: 700px;
  box-shadow: 0 6px 10px rgba(0, 0, 0, 0.15);
  background-color: #ffffff;
}

input {
  border-radius: 8px;
  padding: 12px;
  font-family: Arial, Helvetica, sans-serif;
  border: 1px solid #ccc;
  width: 100%;
  box-sizing: border-box;
}

input:focus {
  outline: none;
  border-color: #007bff;
  box-shadow: 0 0 5px rgba(0, 123, 255, 0.5);
}

.butt {
  display: flex;
  justify-content: center;
  flex-direction: row;
  gap: 10px;
}
</style>


