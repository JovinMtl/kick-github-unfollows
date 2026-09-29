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
        <p>Response here: {{ bestPeople }}</p>
        <!-- <div>
            <ol>    
                <li v-for="person in data">
                    {{ person?.login }}
                </li>
            </ol>
            
        </div> -->
    </div>
    <div>
        <p>and our usernames are: {{ usernames?.length }} <br>unFollows: {{ unFollows }}</p>
        <ol>
            <li :class="user.followStatus == false ? 'bg-red': ''" v-for="user in usernames" :key="user"> {{ user }}</li>
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
    const result = array.map(val=>({username:val.login, followStatus: true}))
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

<style scoped>
.bg-red{
    background-color: red;
}
</style>