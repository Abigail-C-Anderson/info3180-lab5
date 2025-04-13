<template>

    <div v-if="success" class="s-alert">
        <h5> File has Successfully Uploaded </h5>
    </div>

    <div v-if="failure" class="f-alert">
        <ul>
            <li v-for="e in errormsg"> {{e}} </li>
        </ul> 
    </div>

    <form id="movieForm" @submit.prevent="saveMovie">

        <div class="form-group">
            <label for="title" class="form-label">Movie Title</label>
            <br>
            <input id="title" type="text" name="title" class="form-control"/>
            <br>
        </div>   
        <div class="form-group">
            <label for="description" class="form-label2"> Description </label>
            <br>
            <textarea id="description" name="description" rows="5" cols="70"></textarea>
            <br>
        </div> 
        <div class="form-group3">
            <label for="poster" class="form-label3"> Image for Poster </label>
            <input id="poster" type="file" name="poster"/>
            <br>
        </div>
        
        <br><button type="submit" value="submit"> Submit </button>
    
    </form>
</template>

<style>
/* Success message styling */
.s-alert {
    background-color: #4CAF50;
    color: white;
    padding: 20px;
    border-radius: 12px;
    margin-top: 20px;
    text-align: center;
    font-size: 1.2rem;
    font-weight: bold;
}

/* Error message styling */
.f-alert {
    background-color: #F44336;
    color: white;
    padding: 20px;
    border-radius: 12px;
    margin-top: 20px;
    text-align: center;
    font-size: 1.2rem;
    font-weight: bold;
}

/* Movie card styling */
.movie-card {
    display: flex;
    flex-direction: row;
    border-radius: 15px;
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
    padding: 20px;
    margin-bottom: 30px;
    background-color: #fff;
    transition: transform 0.3s ease-in-out;
    overflow: hidden;
}

/* Hover effect on movie cards */
.movie-card:hover {
    transform: translateY(-10px);
}

/* Movie poster styling */
.movie-poster-container {
    flex: 1;
    display: flex;
    justify-content: center;
    align-items: center;
    padding-right: 20px;
}

.movie-poster {
    width: 160px;
    height: 240px;
    object-fit: cover;
    border-radius: 8px;
    border: 4px solid #ddd;
    transition: transform 0.3s ease;
}

/* Hover effect on movie poster */
.movie-poster:hover {
    transform: scale(1.05);
}

/* Movie info styling */
.movie-info {
    flex: 2;
    padding-left: 20px;
}

.movie-info h3 {
    font-size: 1.7rem;
    color: #333;
    margin-bottom: 10px;
    font-weight: bold;
}

.movie-info p {
    font-size: 1rem;
    color: #666;
    line-height: 1.5;
}
</style>

<script setup>
    import {ref, onMounted} from "vue";

    onMounted(() => {
    getCsrfToken();
    success.value = false;
    failure.value = false;
    })

    let csrf_token = ref("");
    let success = ref(false);
    let failure = ref(false);
    let errormsg = ref("");

    function getCsrfToken(){
        
        fetch('/api/v1/csrf-token')
            .then((response) => response.json())
            .then((data) => {
                console.log(data);
                csrf_token.value = data.csrf_token;
            })
    }

    function saveMovie(){
        let movieForm = document.getElementById('movieForm');
        let form_data = new FormData(movieForm);

        fetch("/api/v1/movies", {
            method:'POST',
            body: form_data,
            headers:{
                'X-CSRFTOKEN': csrf_token.value
            }
        })

        .then(function(response){
            return response.json();
        })

        .then(function(data){
            //display a success message
            let info = 0;
            for(info in data){errormsg = data[info];}
            if(document.getElementById('title').value == "" ||
            document.getElementById('description').value == "" || 
            document.getElementById('poster').value ==""){
                failure.value = true;
            }else{success.value = true}
        })

        .catch(function(error){
            console.log(error);
        });
    }

</script>