<template>
    <div>
        It's a honnor to have you here.
        <!-- <nuxtPage/> -->
        <br>
        <label>What is your token</label>&nbsp;
        <input v-model="userToken"/>&nbsp;
        <button @click="getFollowings">Search</button>&nbsp;
        <button @click="checkFollow">Check</button>
    </div>
    <div>
        <p>and our usernames are: {{ usernames?.length }} <br>unFollows: {{ unFollows }}</p>
            <div style="display: flex; align-items: center; margin: 0.5rem;"  v-for="(user, index) in usernames" :key="user"> 
                {{ index+1 }}.&nbsp;
                <NuxtImg 
                    width="48"
                    height="48" 
                    :src="user.avatar_url" 
                    :placeholder="[50, 25]" 
                    style="border-radius: 48px;"
                />&nbsp;
                {{ user.username }} &nbsp;
                <button class="bg-red" style="padding: 0.3rem;    border-radius: 0.3rem;" v-if="user.followStatus == false">unFollow</button>
            </div>
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
const unFollows = ref<string[]>([])
const bestPeople = ref(0)


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
    const result = array.map(val=>({username:val.login, followStatus: true, avatar_url:val.avatar_url}))
    usernames.value = usernames.value.concat(result)
}
function builUnFollows (username:string, promisedValue){
    // const array1 = [1,2,3,5]
    // unFollows.value.push(username)

    // usernames.value.forEach((user)=>{
    //     if((user.username) == username){
    //         console.log("Found :", username)
    //         user.
    //     }
    // })
    const updatedUsername = usernames.value.map(user=>user.username == username ? {...user, followStatus:promisedValue} : user)
    usernames.value = updatedUsername
}

async function doesFollow(user){
    // const oneTimeResponse = ref(true)
        data.value = []
        const url = "https://api.github.com/users/"
        console.log("Running for : ", user?.username)
    try{
        const response = await fetch(`${url}${user?.username}/following/JovinMtl`, {
            method: "GET",
            headers: {
                "Content-type": "application/json",
                Authorization: "Bearer " + userToken.value,
            }
        });
        data.value = await response.json();
        if(response.ok){
            console.log(user?.username,"the Status is okay: ", response)
        } else {
            console.log(user?.username,"The response is not Okay: ", response)
            // unFollows.value.push(user?.username)
            builUnFollows(user?.username, false)
        }
        // if((response.status == 204)){
        //     console.log(user?.username, " it's OKAY. ")
        //     unFollows.value.push(username)
        //     // builUnFollows(user?.username)
            
        // } else if((response.status == 404)){
        //     console.log(user?.username, " Does not follow me ")
        // } else {
        //     console.log(user?.username, " don't know if follows")
        // }
    } catch(e){
        console.log("didn't find the user : ", e)
    }
}
function checkFollow(){
    let counter = 0
    console.log("based on : ", usernames?.value)
    usernames?.value?.forEach(user => {
        doesFollow(user)
    });
    // usernames?.value?.map((user)=>doesFollow(user))
}
</script>

<style>
html{
    background-color: #97e3cb;
    padding: 0;
    margin: 0;
}
body{
    padding: 16px;
}
.bg-red{
    background-color: #d55454;
}
</style>