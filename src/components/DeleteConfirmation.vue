<template>
    <div class="modal">
        <div class="modalContent">
            <p>Are you sure you want to delete this post?</p>
        </div>
        <div class="modalButtons">
            <div class="btn btn1" @click="$emit('confirm')">ok</div>
            <div class="btn" @click="$emit('cancel')">cancel</div>
        </div>
    </div>
</template>

<script setup>
</script>

<style scoped>
    .modal {
        position: fixed;
        top: 45%;
        left: 50%;
        transform: translate(-50%, -50%);
        width: 500px;
        height: 200px;
        border: 0.2px solid #6200EA;
        border-radius: 5px;
        background: white;
        padding: 40px;
        z-index: 1000;
    }
    .modal:hover{
        border: none;
        box-shadow: 1px 1px 4px red;
    }
    .modalContent{
        font-size: 20px;
        font-weight: 300;   
        margin-left: 26px;
    }
    .modalButtons{
        display: flex;
        gap: 20px;
        justify-content: end;
        margin-top: 55px;
    }
    .btn{
        border-radius: 0.5rem;
        background-color: #5d5d63;
        padding: 0.375rem 1.25rem 0.375rem 1.25rem;
        text-transform: uppercase;
        letter-spacing: 0.05em;
        color: #ffff;
        box-shadow: 2px 2px 4px #8b8b9b;
    }
    .btn1{
        background-color: red;
    }
    .btn:hover{
        background-color: #6200EA;
        transition: all 0.3s;
    }
    @media(max-width: 768px){
        .modal{
            width: 350px;
            height: 150px;
            padding: 10px;
        }
        .modalContent{
            font-size: 16px;
            font-weight: 300;
            margin-top: 15px;
            margin-left: 14px;
        }
        .modalButtons{
            margin-top: 42px;
            margin-right: 20px;
        }
        .btn{
            padding: 5px 15px;
            letter-spacing: 0;
            font-size: 15px;
        }
        .btn1{
        background-color: red;
    }
}
</style>