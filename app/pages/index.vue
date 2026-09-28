<template>
    <div>
        It's a honnor to have you here.
        <!-- <nuxtPage/> -->
        <br>
        <label>What is your token</label>
        <input v-model="userToken"/>
        <button @click="getFollowings">Search</button>
        <button @click="checkFollow">Check</button>
    </div>
    <div>
        <p>Response here:</p>
        <!-- <div>
            <ol>    
                <li v-for="person in data">
                    {{ person?.login }}
                </li>
            </ol>
            
        </div> -->
    </div>
    <div>
        <p>and our usernames are: </p>
        <ol>
            <li v-for="user in usernames" :key="user"> {{ user }}</li>
        </ol>
    </div>
</template>

<script setup lang="ts">
import {  ref, toValue } from 'vue'
import token from '../sharedCode/secret'

// States
const pageNumber = ref(1)
const userToken = ref(token)

const data = ref([])
const usernames = ref ([])

// Functions
async function getFollowings(pNumber:number){
        const url = "https://api.github.com/user/following?page="
        const response = await fetch(`${url}${pNumber}`, {
            method: "GET",
            headers: {
                "Content-type": "application/json",
                Authorization: "Bearer " + userToken.value,
            }
        });
        data.value = await response.json();
        if((response.ok)){
            console.log("The reponse we get is : ", response)
            buildUsername(data.value)
            if(data.value.length >= 1){
                pageNumber.value += 1
                console.log("Gotten.")
                getFollowings(pageNumber.value)
            } else{
                console.log("Out of reach")
            }
            
            // getFollowers()
            // console.log("and our user")
        } else{
            // data.value = ["THe response is not OK", ]
                }
}
function buildUsername (array){
    // const array1 = [1,2,3,5]
    const result = array.map(val=>({username:val.login, followsMe: false}))
    usernames.value = usernames.value.concat(result)
}

async function doesFollow(user){
        const url = "https://api.github.com/user/following/"
        console.log("Running for : ", user?.username)
        try{
        const response = await fetch(`${url}${user?.username}`, {
            method: "GET",
            headers: {
                "Content-type": "application/json",
                Authorization: "Bearer " + userToken.value,
            }
        });
        data.value = await response.json();
        if((response.ok)){
            console.log("The reponse we get is : ", data.value)
            
        } else{
            console.log("Failed with reponse : ", data.value)
        }
    } catch(e){
        console.log("didn't find the user : ", e)
    }
}
function checkFollow(){
    let counter = 0
    console.log("based on : ", usernames?.value)
    usernames?.value?.forEach(user => {
        if (counter < 5){
            doesFollow(user)
        }
        counter += 1
    });
}
</script>