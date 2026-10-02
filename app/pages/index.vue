<template>
  <div class="min-h-screen flex flex-col bg-[#5dd8be] font-mono">
    <!-- I hereby, admit on my honor that i was assisted by AI (Claude sonnet 4.6) on refactoring/improving the UI . the original UI can be found in backup/index.vue. October 1, 2026 -->

    <!-- ═══════════════════ NAVBAR ═══════════════════ -->
    <nav class="bg-[#0d0d0d] px-6 py-4 flex items-center justify-between">
      <!-- Logo -->
      <div class="flex items-center gap-3">
        <!-- Terminal icon (mobile only) -->
        <div class="md:hidden w-9 h-9 border border-[#5dd8be] rounded flex items-center justify-center">
          <span class="text-[#5dd8be] text-xs font-bold">&gt;_</span>
        </div>
        <span class="text-[#5dd8be] font-bold text-sm tracking-tight">kick-github-unfollows</span>
      </div>

      <!-- Desktop nav links -->
      <div class="hidden md:flex items-center gap-8">
        <a href="#" class="text-[#5dd8be] font-bold text-sm tracking-wide">dashboard</a>
        <a href="#" class="text-[#4a7a72] text-sm tracking-wide hover:text-[#5dd8be] transition-colors">history</a>
        <a href="#" class="text-[#4a7a72] text-sm tracking-wide hover:text-[#5dd8be] transition-colors">settings</a>
        <a href="#" class="text-[#4a7a72] text-sm tracking-wide hover:text-[#5dd8be] transition-colors">about</a>
      </div>

      <!-- Mobile hamburger -->
      <button class="md:hidden w-9 h-9 border border-[#5dd8be] rounded flex flex-col items-center justify-center gap-1.5">
        <span class="block w-4 h-px bg-[#5dd8be]"></span>
        <span class="block w-4 h-px bg-[#5dd8be]"></span>
        <span class="block w-4 h-px bg-[#5dd8be]"></span>
      </button>
    </nav>

    <!-- Mobile tab bar -->
    <div class="md:hidden bg-[#1a1a1a] flex items-center border-b border-[#2a2a2a]">
      <a href="#" class="px-4 py-3 text-[#0d0d0d] bg-[#5dd8be] text-xs font-bold tracking-wide rounded-sm mx-2 my-2">dashboard</a>
      <a href="#" class="px-4 py-3 text-[#4a7a72] text-xs tracking-wide">history</a>
      <a href="#" class="px-4 py-3 text-[#4a7a72] text-xs tracking-wide">settings</a>
      <a href="#" class="px-4 py-3 text-[#4a7a72] text-xs tracking-wide">about</a>
    </div>

    <!-- ═══════════════════ MAIN CONTENT ═══════════════════ -->
    <main class="flex-1 px-6 py-10 md:px-12 md:py-14 max-w-screen-xl mx-auto w-full">

      <!-- Hero heading -->
      <h1 class="text-[#0d0d0d] font-black text-4xl md:text-6xl tracking-tight leading-none mb-3">
        honor to have you here.
      </h1>
      <p class="text-[#0d0d0d]/70 text-sm md:text-base tracking-wide mb-10">
        find out who has been playing by unfollowing you.
      </p>

      <!-- Cards container -->
      <div class="flex flex-col md:flex-row gap-6">

        <!-- CONNECT CARD -->
        <div class="bg-[#111111] rounded-2xl p-6 flex-1 flex flex-col gap-5">
          <!-- Card header -->
          <div class="flex items-center justify-between">
            <span class="text-[#5dd8be] font-bold text-base tracking-wide">connect</span>
            <span class="w-3 h-3 rounded-full"
              :class="endLoading === 2 ? 'bg-[#5dd8be]' : 'bg-[#3a3a3a]'">
            </span>
          </div>

          <!-- GitHub Token field -->
          <div class="flex flex-col gap-2">
            <label class="text-[#5a8a80] text-xs tracking-wide">github token</label>
            <div class="relative">
              <input
                id="github-token-input"
                :type="showToken ? 'text' : 'password'"
                v-model="userToken"
                placeholder="••••••••••••••••••••••"
                class="w-full bg-transparent border border-[#2a2a2a] rounded-lg px-4 py-3 text-[#5dd8be] text-sm placeholder-[#3a3a3a]/60 tracking-widest outline-none focus:border-[#5dd8be]/50 transition-colors pr-12"
              />
              <!-- Toggle button (mobile) -->
              <button
                @click="showToken = !showToken"
                class="md:hidden absolute right-3 top-1/2 -translate-y-1/2 text-[#4a7a72] hover:text-[#5dd8be] transition-colors"
              >
                <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                  <path v-if="showToken" stroke-linecap="round" stroke-linejoin="round" stroke-width="1.5"
                    d="M13.875 18.825A10.05 10.05 0 0112 19c-4.478 0-8.268-2.943-9.543-7a9.97 9.97 0 011.563-3.029m5.858.908a3 3 0 114.243 4.243M9.878 9.878l4.242 4.242M9.88 9.88l-3.29-3.29m7.532 7.532l3.29 3.29M3 3l3.59 3.59m0 0A9.953 9.953 0 0112 5c4.478 0 8.268 2.943 9.543 7a10.025 10.025 0 01-4.132 5.411m0 0L21 21"/>
                  <path v-else stroke-linecap="round" stroke-linejoin="round" stroke-width="1.5"
                    d="M15 12a3 3 0 11-6 0 3 3 0 016 0z M2.458 12C3.732 7.943 7.523 5 12 5c4.478 0 8.268 2.943 9.542 7-1.274 4.057-5.064 7-9.542 7-4.477 0-8.268-2.943-9.542-7z"/>
                </svg>
              </button>
              <!-- Lock icon (desktop) -->
              <svg class="hidden md:block absolute right-3 top-1/2 -translate-y-1/2 w-4 h-4 text-[#4a7a72]" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="1.5" d="M12 15v2m-6 4h12a2 2 0 002-2v-6a2 2 0 00-2-2H6a2 2 0 00-2 2v6a2 2 0 002 2zm10-10V7a4 4 0 00-8 0v4h8z"/>
              </svg>
            </div>
            <div class="flex items-center gap-2">
              <svg class="w-3.5 h-3.5 text-[#4a7a72]" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="1.5" d="M9 12l2 2 4-4m5.618-4.016A11.955 11.955 0 0112 2.944a11.955 11.955 0 01-8.618 3.04A12.02 12.02 0 003 9c0 5.591 3.824 10.29 9 11.622 5.176-1.332 9-6.03 9-11.622 0-1.042-.133-2.052-.382-3.016z"/>
              </svg>
              <span class="text-[#4a7a72] text-[11px] tracking-wide">token is never stored or exposed</span>
            </div>
          </div>

          <!-- GitHub Username field -->
          <div class="flex flex-col gap-2">
            <label class="text-[#5a8a80] text-xs tracking-wide">github username</label>
            <input
              id="github-username-input"
              type="text"
              v-model="userName"
              placeholder="your-username"
              class="w-full bg-transparent border border-[#2a2a2a] rounded-lg px-4 py-3 text-[#5dd8be] text-sm placeholder-[#3a3a3a] outline-none focus:border-[#5dd8be]/50 transition-colors"
            />
          </div>

          <!-- Find unfollowers button -->
          <button
            id="find-unfollowers-btn"
            @click="getFollowings"
            class="w-full bg-[#5dd8be] hover:bg-[#4ecaae] text-[#0d0d0d] font-bold text-sm tracking-widest py-4 rounded-xl transition-colors flex items-center justify-center gap-2"
          >
            <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z"/>
            </svg>
            find unfollowers
          </button>

          <!-- Unfollow all button -->
          <button
            v-if="endLoading == 2"
            id="unfollow-all-btn"
            @click="checkUsernameValid(userName, 1)"
            class="w-full bg-transparent border border-[#5dd8be] hover:bg-[#5dd8be]/10 text-[#5dd8be] font-bold text-sm tracking-widest py-4 rounded-xl transition-colors flex items-center justify-center gap-2"
          >
            <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M17 16l4-4m0 0l-4-4m4 4H7m6 4v1a3 3 0 01-3 3H6a3 3 0 01-3-3V7a3 3 0 013-3h4a3 3 0 013 3v1"/>
            </svg>
            unfollow all
          </button>

          <!-- Status line -->
          <div v-if="endLoading > 0" class="flex items-center gap-2">
            <span class="w-2.5 h-2.5 rounded-full flex-shrink-0"
              :class="endLoading === 2 ? 'bg-[#5dd8be]' : 'bg-[#f59e0b] animate-pulse'">
            </span>
            <span class="text-[#5a8a80] text-xs tracking-wide">
              <template v-if="endLoading === 1">scanning... {{ usernames?.length }} found</template>
              <template v-else-if="endLoading === 2">
                {{ usernames?.length }} followings found &mdash; done
              </template>
            </span>
          </div>
        </div>

        <!-- UNFOLLOWERS CARD -->
        <div 
          class="bg-[#111111] rounded-2xl p-6 flex-[1.6] flex flex-col gap-4 min-h-[360px] max-h-[75vh] overflow-auto jove">
          <!-- Card header -->
          <div class="flex items-center justify-between">
            <span class="text-[#5dd8be] font-bold text-base tracking-wide">unfollowers</span>
            <span v-if="badPeople > 0"
              class="bg-[#5dd8be] text-[#0d0d0d] text-xs font-black px-3 py-1 rounded">
              {{ badPeople }}
            </span>
          </div>

          <!-- Unfollower list -->
          <div class="flex flex-col gap-3 flex-1 overflow-y-auto">
            <template v-if="usernames?.length === 0 && endLoading === 0">
              <div class="flex-1 flex items-center justify-center">
                <p class="text-[#2a2a2a] text-xs tracking-wide">no data yet — run a search</p>
              </div>
            </template>

            <div
              v-for="(user, index) in unFollowers"
              :key="user.username"
              class="flex items-center gap-3"
            >
              <!-- Avatar -->
              <NuxtImg
                v-if="user.avatar_url"
                :src="user.avatar_url"
                width="44"
                height="44"
                :placeholder="[44, 44]"
                class="rounded-full flex-shrink-0 w-11 h-11 object-cover"
              />
              <!-- Skeleton avatar -->
              <div v-else class="w-11 h-11 rounded-full bg-[#2a2a2a] flex-shrink-0 animate-pulse"></div>

              <!-- Name & status -->
              <div class="flex-1 min-w-0">
                <template v-if="user.username">
                  <p class="text-white font-bold text-sm tracking-wide truncate">{{ user.username }}</p>
                  <p class="text-[#4a7a72] text-xs tracking-wide">
                    <span v-if="user.followStatus === false">unfollowed you</span>
                    <span v-else>follows you</span>
                </p>
                </template>
                <template v-else>
                  <div class="h-3 bg-[#2a2a2a] rounded w-28 mb-1.5 animate-pulse"></div>
                  <div class="h-2.5 bg-[#2a2a2a] rounded w-20 animate-pulse"></div>
                </template>
              </div>

              <!-- Remove button (non-followers) -->
              <button
                v-if="user.followStatus === false"
                @click="checkUsernameValid(user.username, 2)"
                class="flex-shrink-0 border border-[#2a2a2a] text-[#8a8a8a] text-[#ef4444] hover:border-[#5dd8be] hover:text-[#5dd8be] text-xs px-3 py-1.5 rounded tracking-wide transition-colors"
              >
                remove
              </button>
              <!-- Scanning label -->
              <span v-else-if="endLoading === 1" class="text-[#2a2a2a] text-xs tracking-wide flex-shrink-0">scanning...</span>
            </div>
          </div>

          <!-- Rate limit banner (controlled by your logic — bind v-if to your rate-limit state) -->
          <div class="bg-[#2a0a0a] border border-[#5a1a1a] rounded-lg px-4 py-3 flex items-center justify-between" style="display:none">
            <div class="flex items-center gap-2">
              <span class="w-2 h-2 rounded-full bg-[#ef4444] flex-shrink-0"></span>
              <span class="text-[#ef4444] text-xs tracking-wide font-mono">rate limit &mdash; retry in 60s</span>
            </div>
            <button class="text-[#5dd8be] text-xs font-bold tracking-wide hover:underline">retry</button>
          </div>
        </div>

      </div><!-- end cards -->
    </main>

    <!-- ═══════════════════ FOOTER ═══════════════════ -->
    <footer class="bg-[#e8f5f1] px-6 py-5 md:px-12">
      <div class="max-w-screen-xl mx-auto flex flex-col md:flex-row items-center justify-between gap-2">
        <span class="text-[#4a7a72] text-xs tracking-wide">2026</span>
        <span class="text-[#4a7a72] text-xs tracking-wide">Thierry Nsanzumukiza</span>
        <span class="text-[#4a7a72] text-xs tracking-wide">copyright &mdash; reserved.</span>
      </div>
    </footer>

  </div>
</template>

<script setup lang="ts">
import {  ref, toValue, watch } from 'vue'
import type { Ref } from 'vue'

// States
const pageNumber:Ref<number> = ref(1)
const userToken:Ref<string> = ref('')
const userName:Ref<string> = ref('')
const showToken:Ref<boolean> = ref(false)

const data = ref([])
const usernames = ref ([])
const unFollows = ref<string[]>([])
const badPeople:Ref<number>  = ref(0)
const endLoading:Ref<number>  = ref(0)

const validUsername:Ref<string> = ref('')
const commandNumber:Ref<number> = ref(0)

const unFollowers = ref([])


// Functions
async function getFollowings(pNumber:number){
    badPeople.value = 0
    if(endLoading.value == 2){
        usernames.value = [];
        pageNumber.value = 1
    }
    endLoading.value = 1;
        const url = "https://api.github.com/user/following?page="
        const perPage = '&per_page=100'
        const response = await fetch(`${url}${pNumber}${perPage}`, {
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
    const result = array.map(val=>({username:val.login, followStatus: true, avatar_url:val.avatar_url, checked:false}))
    usernames.value = usernames.value.concat(result)
}
function builUnFollows (username:string, promisedValue){
    const updatedUsername = usernames.value.map(user=>user.username == username ? {...user, followStatus:promisedValue} : user)
    usernames.value = updatedUsername
    // usernames.value.forEach((user)=>{
    //   user.checked = true
    //   if(user.)
    // })
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
        // unFollowPerson()
    } else if (commandNumber.value == 2){
        unFollowPerson()
    }
})
watch(endLoading, (newValue)=>{
  if(newValue == 2){
    // Run Check of follow-back function.
    checkFollow()
  }
})
watch(usernames, (newValue)=>{
  unFollowers.value = usernames?.value?.filter(user=>user?.followStatus == false)
  console.log("unFollowers : " + unFollowers.value + " from " + newValue)
})
</script>

<style scoped>
  ::-webkit-scrollbar-thumb {
    background-color: black;
    background-color: #5dd8be;
    border-radius: 15px;
    color : #5dd8be;
  }
 ::-webkit-scrollbar:horizontal{
    width: 2px;
  }
 ::-webkit-scrollbar {
    width: 4px;
    width: 8px;   
    /*height: 3px;*/
    color: green;
  }
  ::-webkit-scrollbar-track {
    background-color: transparent;
  }
</style>