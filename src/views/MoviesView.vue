<script setup>
 import { ref, onMounted } from "vue";
    let mov = ref([]);
    onMounted(() => {
        fetchMovies()
    })

    function fetchMovies(){
    
    fetch('/api/v1/movies',{
        method:'GET'
    })
      .then(function(response) {
        return response.json();
        })
      .then(function(data) {
        mov.value = data.mov;
      })
      .catch(function(error){
         console.log(error);
      })
    }
</script>


<template>
        <h2 class="head"> Movies </h2>
        <div class="cards">
            <div v-for="movie in mov" class="card">
                <div class="image">
                    <img :src="movie.poster"/>
                </div>
                <div class="content">
                    <h3> {{movie.title}} </h3>
                    <p> {{movie.description}} </p>
                </div>
            </div>
        </div>
</template>

<style>
/* Main container for the cards */
.cards {
    display: flex;
    flex-wrap: wrap;
    justify-content: space-around;
    padding: 20px;
    gap: 20px;
}

/* Each card */
.card {
    display: flex;
    flex-direction: column;
    border-radius: 12px;
    width: 300px;
    box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
    background-color: #fff;
    overflow: hidden;
    transition: transform 0.3s ease, box-shadow 0.3s ease;
    cursor: pointer;
}

/* Hover effect for the cards */
.card:hover {
    transform: translateY(-10px);
    box-shadow: 0 8px 15px rgba(0, 0, 0, 0.2);
}

/* Card image styling */
.image img {
    width: 100%;
    height: 200px;
    object-fit: cover;
    border-bottom: 2px solid #ddd;
}

/* Content inside the card */
.content {
    padding: 15px;
}

/* Movie title styling */
.content h3 {
    font-size: 1.4rem;
    color: #333;
    margin-bottom: 10px;
    font-weight: bold;
}

/* Movie description styling */
.content p {
    font-size: 1rem;
    color: #666;
    line-height: 1.5;
    font-weight: 400;
}

/* Header styling */
.head {
    text-align: center;
    font-size: 2rem;
    color: #333;
    margin-bottom: 30px;
}

/* Responsive styling for smaller screens */
@media (max-width: 768px) {
    .cards {
        justify-content: center;
    }
    
    .card {
        width: 250px;
    }
}
</style>