<template>
<div>
    <Main @blockItemInfo="blockItem" />
        <div v-if="isEditModalActive == true" class='modal-background' @click="closeModal"></div>
    <transition name="modal-fade">
        <form v-if="isEditModalActive == true" action="submit">
            <div @click="closeModal" class='close-x'>&times;</div>
            <h1 v-if="isVerifying" class='loadingText'>Awaiting Verification...</h1>
            <h1 v-if="isVerified" class='loadingTextSpan'>Verification Complete!</h1>
            <div v-if="isVerifying" class='loading'></div>
            <span v-if="isVerifying" class='loadingSpan'></span>
            <h1 v-if="hasNotVerified">Edit Description For: <span class='modal-block-number'> {{ blockItem.meta.name }} </span></h1>
            <input v-if="hasNotVerified" :placeholder="blockItem.meta.description" v-model="editDescriptionInput" @input="editAttempt" />
            <button v-if="hasNotVerified" @click="submitEdit">Submit</button>
        </form>
    </transition>
</div>
</template>

<script>
// import Main from '@/components/Main.vue'

export default {
    name: 'EditModal',
    props: ['blockItemInfo'],
    data() {
        return {
            isEditModalActive: false,
            blockItem: null,
            editDescriptionInput: '',
            submittedDescriptionText: '',
            hasNotVerified: false,
            isVerifying: false,
            isVerified: false
        }
    },
    methods: {
        editDescription(blockItemInfo) {
            this.blockItem = blockItemInfo;
            this.isEditModalActive = true;
            this.hasNotVerified = true;
        },
        editAttempt(e) {
            this.editDescriptionInput = e.target.value;
            this.hasNotVerified = true;
        },
        submitEdit(e) {
            e.preventDefault();
            this.submittedDescriptionText = this.editDescriptionInput;
            this.$emit('newDescriptionSubmitted', this.submittedDescriptionText)
            this.isVerifying = true
            this.hasNotVerified = false;
            this.editDescriptionInput = ''
        },
        closeModal() {
            this.hasNotVerified = true,
                this.isEditModalActive = false,
                this.isVerifying = false
            this.isVerified = false;
        },
        closeModalAfterVerification() {
            this.isVerified = true;
            this.isVerifying = false;
            setTimeout(() => {
                this.triggerVerifiedClose()
            }, 2000);
        },
        triggerVerifiedClose() {
            this.isEditModalActive = false,
                this.isVerified = false;
            this.hasNotVerified = true,
                this.isEditModalActive = false,
                this.isVerifying = false
            this.isVerified = false;
        },
    }
}
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Roboto:wght@400;500&display=swap');

html,
body,
h1,
#main-box-container {
    font-family: 'Roboto';
}

.modal-fade-enter,
.modal-fade-leave-to {
/* opacity: 0; */
transform: translate(0%, 0%) scale(1);
}

.modal-fade-enter-active,
.modal-fade-leave-active {
transform: translate(0%, 0%) scale(0);
}

p {
    font-size: 14px;
    color: #939fb9;
}

form {
    position: fixed;
    max-width: 80%;
    height: 180px;
    top: 30%;
    left: 30%;
    padding: 20px 20px;
    border: 0.5px solid rgba(175, 175, 175, 0.115);
    border-radius: 20px;
    box-shadow: 5px 5px 20px rgba(0, 0, 0, 0.32);
    background-color: white;
    z-index: 10;
    transition: 300ms ease-in-out;
    display: flex;
    flex-direction: column;
    justify-content: space-evenly;
    align-items: center;
}

.close-x {
    position: absolute;
    top: 2.5%;
    right: 3%;
    width: 100%;
    cursor: pointer;
    text-align: right;
    font-size: 24px;
}

.close-x:hover {
    color: #0bd9ba;
}

h1 {
    color: #343d53;
    font-size: 13px;
}

input {
    width: 510px;
    height: 40px;
    border-radius: 10px;
    border: 1px solid #D7DAE3;
    padding-left: 8px;
    border-radius: 8px;
    color: #192133;
}

input:focus {
    color: #0b1a40;
    outline: none;
}

input::placeholder {
    color: #939FB9;
    opacity: 0.7;
    font-size: 12px;
}

button {
    width: 140px;
    height: 35px;
    border-radius: 8px;
    border: none;
    background-color: #05c0a5;
    color: white;
    font-size: 12px;
}

button:hover {
    transition: ease-in-out 200ms;
    cursor: pointer;
    background-color: #0bd9ba;
}

.modal-block-number {
    color: #05c0a5;
}

.modal-background {
    opacity: 1;
    position: fixed;
    /* Stay in place */
    z-index: 9;
    /* Sit on top */
    left: 0;
    top: 0;
    right: 0;
    bottom: 0;
    background-color: rgba(0, 0, 0, 0.4);
    /* Black w/ opacity */
    transition: 300ms ease-in-out;
    pointer-events: all;
}

.loadingText {
    height: 80px;
}

.loadingTextSpan {
    width: 510px;
}

.loading {
    position: absolute;
    width: 50px;
    height: 50px;
    margin-top: 20px;
    border: 6px solid #D7DAE3;
    border-radius: 50%;
    border-top: solid 5px #05C0A5;
    animation: loading 2s infinite linear;
}

.loadingSpan {
    width: 510px;
}

@keyframes loading {
    0% {
        transform: rotate(0deg)
    }

    100% {
        transform: rotate(360deg)
    }
}
</style>
