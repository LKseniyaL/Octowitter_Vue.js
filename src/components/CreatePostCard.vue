<template>
    <div>
        <div class="containerCreate">

            <!-- content length start -->
            <p class="absolute top-2 right-2 text-xs text-zinc-400 select-none" :class="{'text-violet-700 font-bold' : newPost.content.length>=180}">{{newPost.content.length}}/180</p>
            <!-- content length end -->

            <!-- clear input start -->
            <div @click="resetInput" class="absolute top-8 right-2 text-zinc-400 hover:text-violet-700  transition-all cursor-pointer">
                <svg class="" width="16" height="16" fill="none" stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="4" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
                <path d="m5 5 14 14M5 19 19 5"></path>
                </svg>
            </div>
            <!-- clear input end -->



            <!-- text input start -->
            <textarea 
                maxlength="180" 
                class="textarea no-scrollbar" 
                @keydown.enter.prevent 
                name="content" 
                id="content" 
                placeholder="Your text here" 
                autocomplete="off" 
                spellcheck="false"
                v-model="newPost.content"></textarea>
            <!-- text input end -->

            <!-- buttons nav start-->
            <div class="buttonsContainer">
                <!-- attach button start -->
                <div class="buttonSvg">
                    <svg width="24" height="24" fill="none" stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="1" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
                    <path d="M6 7.91V16a6 6 0 0 0 6 6v0a6 6 0 0 0 6-6V6a4 4 0 0 0-4-4v0a4 4 0 0 0-4 4v9.182a2 2 0 0 0 2 2v0a2 2 0 0 0 2-2V8"></path>
                    </svg>
                </div>
                <!-- attach button end -->

                <!-- emote button start -->
                <div class="buttonSvg">
                    <svg width="24" height="24" fill="none" stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="1" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
                    <path d="M12 2a10 10 0 1 0 0 20 10 10 0 1 0 0-20z"></path>
                    <path d="M8 9.05v-.1"></path>
                    <path d="M16 9.05v-.1"></path>
                    <path d="M16 14c-.5 1.5-1.79 3-4 3s-3.5-1.5-4-3"></path>
                    </svg>
                </div>
                <!-- emote button end -->

                <!-- submit button start -->
                <button @click="addNewPost" class="buttonSubmit" :class="{'!saturate-0 !pointer-events-none !cursor-default' : newPost.content == ''}">SUBMIT</button>
                <!-- submit button end -->
            </div>
            <!-- buttons nav end-->
        </div>
        <div class="btnsBlock">
            <div @click="clearChat" class="btnClear">Clear Chat</div>
            <div @click="toggleChat" class="btnStop">Start/Stop</div>
        </div>
    </div>
</template>

<script setup>
    import { postData } from "../postData";
    import { onMounted, onUnmounted, reactive } from "vue";
    import  store  from "/src/store";


    const newPost = reactive({
        postid:"",
        content:"",
        uid:"6atuhan",
        date:"",
        like:0,
        dislike:0,
        isLike:false,
        isDislike:false
    })


    //generate postid
    const generateUniqueId =()=> {
    return Math.random().toString(36).substring(2);
    }

    //clear input
    const resetInput =()=>{
        newPost.content=""
        newPost.postid=""
        newPost.date=""

    }

    // push to store 
let deleteIntervals = [];

const addNewPost = () => {
    if(newPost.content != ""){
        const copyOfNewPost = {
            ...newPost,
            postid: generateUniqueId(),
            date: Date.now()
        };
        store.dispatch("addPostAction", copyOfNewPost);
        localStorage.setItem('store', JSON.stringify(store.getters.getPosts));


        const deleteInterval = setTimeout(() => {
            store.dispatch("deletePostAction", copyOfNewPost);
            localStorage.setItem('store', JSON.stringify(store.getters.getPosts));
        }, 9000);
        
        deleteIntervals.push(deleteInterval);

        resetInput();
    }
};


    // automatic creation
    const autoAddPost = () => {
        const randomIndex = Math.floor(Math.random() * postData.length);
        newPost.content = postData[randomIndex].content;
        newPost.uid = postData[randomIndex].uid;
        addNewPost();
    };

    // adding and removing intervals
let interval;
const toggleChat = () => {
    if (store.state.isIntervalRunning) {
        clearInterval(interval);
        deleteIntervals.forEach(clearTimeout);
        deleteIntervals = [];

        store.dispatch("toggleIntervalAction");  
    } else {
        interval = setInterval(autoAddPost, 5000);
        store.dispatch("toggleIntervalAction");
    }
};

onUnmounted(() => {
    clearInterval(interval);
});

        // clear chat
const clearChat = () => {
    store.dispatch("clearChatAction")
};

</script>

<style scoped>
.containerCreate{
    @apply
    md:w-[26rem] w-full mx-2 md:mx-0 
    bg-white
    rounded-md border border-violet-400
    p-6
    drop-shadow-lg
    mb-4
    relative
}

.textarea{
    @apply
    outline-none 
    w-full h-24 
    break-words 
    resize-none 
    px-1.5 py-1
    transition-all duration-500
    placeholder:text-zinc-200 hover:placeholder:text-zinc-400
}

.buttonsContainer{
    @apply
    flex items-center justify-start
    gap-2
}
.buttonSvg{
    @apply
    hover:text-violet-700 hover:fill-violet-700 text-zinc-500 fill-zinc-500 transition-all

}
.buttonSubmit{
    @apply
    uppercase text-white tracking-wider
    px-5 py-1.5
    ml-auto
    rounded-lg
    transition-all
    bg-violet-600 hover:bg-violet-700
    shadow-md shadow-transparent hover:shadow-violet-700/50
    active:bg-violet-500
    select-none
}
.btnsBlock{
    display: flex;
    justify-content: center;
    gap: 80px;
    margin-bottom: 10px;
}
.btnsBlock div{
    cursor: pointer;
    border-radius: 0.5rem;
    background-color: #7c3aed;
    padding: 5px 20px;
    color: #ffff;
    box-shadow: 3px 3px 3px #8b8b9b;
}
.btnsBlock div:hover{
    background-color: #5d10e3;
    transition: all 0.5s;
}
</style>