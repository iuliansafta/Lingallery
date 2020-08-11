<template>
  <div>
    <div
      id="galleryLargeViewContainer"
      :class="{ fadeIn: runAnimation, fadeOut: !runAnimation }"
    >
      <div class="largerGallertWrapper">
        <div class="largeGalleryHoldMyBear">
          <div class="gallery-holder">
            <div class="gallery-image" v-for="(item, index) in photos" :key="index">
              <div class="gallery-button-image" :class="{selected: isSelected(index)}" @click="selectItem(index)">
                <img :src="item.src" />
              </div>
            </div>
          </div>
        </div>
        <div class="largeGalleryHoldMyFooter">
          <button class="btn btn-danger" @click="handleDeletePhotos">Delete {{this.selectedItems.length}} selected photos</button>
        </div>
        <a class="close-button" @click="handleCloseClick"></a>
      </div>   
    </div>
  </div>
</template>

<script >
export default {
  name: 'GalleryLargeView',
  props: {
    items: {
      type: Array,
      default() {
        return []
      }
    },
    accentColor: {
      type: String,
      default: ''
    }
  },
  data() {
    return {
      runAnimation: true,
      isLoading: true,
      selectedItems: [],
      photos: this.items,
    }
  },
  methods: {
    selectItem(index) {
      if (this.isSelected(index)) {
        this.selectedItems.splice(index, 1);
        return;
      }

      this.selectedItems.push(index);
    },
    isSelected(id) {
      return this.selectedItems.includes(id);
    },
    handleCloseClick() {
      this.runAnimation = false
      window.setTimeout(() => {
        this.runAnimation = true
        this.$emit('close-large-view')
      }, 500)
    },
    handleDeletePhotos() {
      const items = [];
      this.selectedItems.forEach((index) => {
        const item = this.photos[index];
        items.push(item);
        this.photos.splice(index, 1);
      });

      this.selectedItems = [];
      this.$emit('delete-selected-photos', items);
    }
  }
}
</script>

<style lang="scss" scoped>
#galleryLargeViewContainer {
  z-index: 999;
  position: fixed;
  display: grid;
  justify-items: center;
  align-items: center;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  background-color: rgba(0, 0, 0, 0.75);
  &.fadeIn,
  &.fadeOut {
    animation-name: fadeIn;
    animation-fill-mode: forwards;
    animation-duration: 0.5s;
  }
  &.fadeOut {
    animation-name: fadeOut;
  }
  .close-button {
    position: absolute;
    top: -30px;
    right: -30px;
    display: block;
    width: 32px;
    height: 32px;
    opacity: 0.5;
    cursor: pointer;
    &:hover,
    &:focus {
      opacity: 1;
    }
    &:before,
    &:after {
      position: absolute;
      left: 15px;
      content: ' ';
      height: 33px;
      width: 2px;
      background-color: #fff;
    }
    &:before {
      transform: rotate(45deg);
    }
    &:after {
      transform: rotate(-45deg);
    }
  }

  .largerGallertWrapper {
    background: #fff;
    padding: 20px;
    width: 780px;
    height: 700px;
    position: relative;
  }
  .largeGalleryHoldMyBear {
    overflow: auto;
    height: 650px;
  }
  .largeGalleryHoldMyFooter {
    padding: 20px;
  }
  .gallery-holder {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(370px, 1fr));
    grid-auto-rows: 1fr;
    grid-gap: 10px;
    grid-auto-flow: dense;
  }
  .gallery-button-image {
    position: relative;
    overflow: hidden;
    width: 370px;
    height: 370px;
    border: 4px solid #fff;

    &.selected { 
      border-color: #a90329;
    }

    >img {
      position: absolute;
      top: 50%;
      left: 50%; 
      transform: translate(-50%,-50%)
    }
  }
}
@keyframes fadeIn {
  from {
    opacity: 0;
  }

  to {
    opacity: 1;
  }
}
@keyframes fadeOut {
  from {
    opacity: 1;
  }

  to {
    opacity: 0;
  }
}
</style>