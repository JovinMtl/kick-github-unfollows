<template>
    <!-- <div class="stroke-pt"></div> -->
    <div style="background-color: grey;">
    <div>
        <h1>It's an honor to have you here.</h1>
        <h3>Find out who have been playing by unFollowing you.</h3>
        <br>
        <label>Your token</label>&nbsp;
        <input class="inp" v-model="userToken"/><br>
        <label>Username &nbsp;</label>&nbsp;
        <input class="inp" v-model="userName"/><br>
        <button class="inp" @click="getFollowings">Search</button>&nbsp;
        <button class="inp" @click="checkUsernameValid(userName, 1)">Check</button>
    </div>
    <div>
        <p>
            and the people you follow are: {{ usernames?.length }}
            <span v-if="endLoading == 1">still counting...</span> 
            <span v-else-if="endLoading == 2">Done</span>
            <span class="red" v-if="badPeople"> 
                ({{ badPeople }} bad people)</span>
        </p>
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
                <button class="bg-red" style="padding: 0.3rem;    border-radius: 0.3rem;" v-if="user.followStatus == false" @click="checkUsernameValid(user.username, 2)">unFollow</button>
            </div>
    </div>
    </div>
    <div class="stroke-pt" style="z-index: -1;"></div>
</template>

<script setup lang="ts">
import {  ref, toValue, watch } from 'vue'
import type { Ref } from 'vue'

// States
const pageNumber:Ref<number> = ref(1)
const userToken:Ref<string> = ref('')
const userName:Ref<string> = ref('')

const data = ref([])
const usernames = ref ([])
const unFollows = ref<string[]>([])
const badPeople:Ref<number>  = ref(0)
const endLoading:Ref<number>  = ref(0)

const validUsername:Ref<string> = ref('')
const commandNumber:Ref<number> = ref(0)


// Functions
async function getFollowings(pNumber:number){
    badPeople.value = 0
    if(endLoading.value == 2){
        usernames.value = [];
        pageNumber.value = 1
    }
    endLoading.value = 1;
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
                console.log("Out of reach");
                endLoading.value = 2
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
        const response = await fetch(`${url}${user?.username}/following/${userName.value}`, {
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
            badPeople.value += 1
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
    badPeople.value = 0
    console.log("based on : ", usernames?.value)
    usernames?.value?.forEach(user => {
        doesFollow(user)
    });
    return 0
}
async function checkUsernameValid(username:string, command:number=0){
    // command points the end use of this function
    // 0= 'nothing'
    // 1="doesFollow()", 2="unFollowPerson()"
    if (username.length > 2){
        const url = "https://api.github.com/users/"
        console.log("Ask if username ( " + username + " ) is real.")
        try{
            const response = await fetch(`${url}${username}`, {
                method: "GET",
                headers: {
                    "Content-type": "application/json",
                    Authorization: "Bearer " + userToken.value,
                }
            });
            data.value = await response.json();
            if(response.ok){
                console.log(username,"the username is VALID: ", response)
                validUsername.value = username
                commandNumber.value = Number(command)
            } else {
                console.log(username,"The username is not VALID: ", response)
            }
        } catch(e){
            console.log("didn't find the user : ", e)
        }
    } else{
        // notify that caracters must be above 2.
    }   
}
async function unFollowPerson(username){
    const url = "https://api.github.com/user/following/"
    console.log("About to unFollow : ", username)
    // try{
    //     const response = await fetch(`${url}${username}`, {
    //         method: "DELETE",
    //         headers: {
    //             "Content-type": "application/json",
    //             Authorization: "Bearer " + userToken.value,
    //         }
    //     });
    //     data.value = await response.json();
    //     if(response.ok){
    //         console.log(username,"the Status is okay: ", response)
    //     } else {
    //         console.log(username,"The response is not Okay: ", response)
    //         // builUnFollows(username, false)
    //     }
    // } catch(e){
    //     console.log("didn't find the user : ", e)
    // }
}


//Watchers : my favorite place to 
//          take important actions from
//              interaction. (Sep 30, 2026)
watch(validUsername, (newUsername)=>{
    // 1="doesFollow()", 2="unFollowPerson()"
    if (commandNumber.value == 1){
        checkFollow(newUsername)
    } else if (commandNumber.value == 2){
        unFollowPerson()
    }
})
</script>

<style>
html{
    background-color: #97e3cb;
    padding: 0;
    margin: 0;
}
body{
    padding: 16px;
    font-family: monospace;
}
.bg-red{
    background-color: #d55454;
}
.inp{
    padding: 8px;
    margin: 4px;
    border-radius: 4px;
}
.white{
    color: white;
}
.red{
    color: red;
}
.stroke-pt{
            position: absolute;
            width: 103vw;
            height: 110%;

            background-image: url('data:image/svg+xml,<svg xmlns="http://www.w3.org/2000/svg" width="928" height="374" fill="none"><path stroke="bisque" stroke-width="1" d="M341 1s47 16 67 88 88 24 119 37-15 71 23 85 53 31 72 64 102-14 129 28 93 55 93 55l24 8 59 7M311 1s47 16 67 88 88 24 119 37-15 71 23 85 53 31 72 64 102-14 129 28 93 55 93 55l24 8 59 7M279 1s47 16 67 88 88 24 119 37-15 71 23 85 53 31 72 64 102-14 129 28 93 55 93 55l24 8 59 7M247 1s47 16 67 88 88 24 119 37-15 71 23 85 53 31 72 64 102-14 129 28 93 55 93 55l24 8 59 7M69 1s47 16 67 88 88 24 119 37-15 71 23 85 53 31 72 64 102-14 129 28 93 55 93 55l24 8 59 7M35 1s47 16 67 88 88 24 119 37-15 71 23 85 53 31 72 64 102-14 129 28 93 55 93 55l24 8 59 7M1 1s47 16 67 88 88 24 119 37-15 71 23 85 53 31 72 64 102-14 129 28 93 55 93 55l24 8 59 7"/></svg>');

            
            background-size: 115% 115%;
            top: -1.3rem;
            left: -8rem;
            left: -18rem;
            transform: rotate(-3deg);
        }
</style>
