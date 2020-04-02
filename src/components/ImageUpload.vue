<template>
  <div class="upload-container">
        <div
        class="upload-area"
        @drop.prevent.stop="onDrop"
        @dragenter.prevent.stop
        @dragover.prevent.stop
        @dragleave.prevent.stop
    >
        <img v-if="imageUrl" :src="imageUrl" >
    </div>

    <button v-if="files.length" @click="sendImages" class="upload-btn">
        Send
    </button>
  </div>
</template>

<style scoped>
.upload-container {
    display: flex;
    flex-direction: column;
    align-items: center;
}
.upload-area {
    width: 600px;
    height: 400px;
    border: 10px dashed #08E6A2;
    display: flex;
    justify-content: center;
    align-items: center;
}
.upload-area img {
    max-width: 100%;
    max-height: 100%;
}
.upload-btn {
    margin: 30px;
    background-color: #08E6A2;
    border: none;
    width: 160px;
    height: 50px;
    border-radius: 4px;
    color: #fff;
    font-size: 24px;
    cursor: pointer;
    outline: none;
}
.upload-btn:hover {
    background-color: #00DE9A;
}
</style>

<script>
export default {
    data() {
        return {
            imageUrl: "",
            files: [],
        }
    },
    methods: {
        onDrop(event) {
            const { files } = event.dataTransfer;
            const images = [...files].filter(file => {
                const [ type ] = file.type.split('/');
                return type === 'image' && this.isUniq(file);
            });

            this.files.push(...images);
            this.setImagePreview(images);
        },
        setImagePreview(files) {
            if (!files.length) return;

            const reader = new FileReader();
            const lastFile = files[files.length - 1];

            reader.readAsDataURL(lastFile);
            reader.onload = event => this.imageUrl = event.target.result;
        },
        sendImages() {
            const promises = this.files.map(file => new Promise(resolve => {
                const reader = new FileReader();
                reader.onload = event => resolve(event.target.result);
                reader.readAsDataURL(file);
            }));

            Promise.all(promises).then(console.log)
        },
        isUniq({ name }) {
            return !this.files.find(file => file.name === name);
        },
    }
}
</script>
