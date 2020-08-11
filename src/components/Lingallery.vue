<template>
  <div class="lingalleryContainer">
    <div :style="lingalleryStyle" class="lingallery">
      <large-view
        id="largeView"
        :accent-color="accentColor"
        :item="items[currentIndex]"
        v-if="addons.enableLargeView && showLargeView"
        @close-large-view="showLargeView = false"
      />
      <gallery-large-view
        id="galleryLargeView"
        :accent-color="accentColor"
        :items="items"
        v-if="addons.enableGalleryLargeView && showGalleryLargeView"
        @close-large-view="showGalleryLargeView = false"
        @delete-selected-photos="deleteSelectedPhotos"
      />
    
      <figure
        :style="figureStyle"
        itemprop="associatedMedia"
        itemscope
        itemtype="http://schema.org/ImageObject"
      >
        <div class="lingallery_spinner">
          <half-circle-spinner
            :animation-duration="1000"
            :color="accentColor"
            :size="60"
            v-if="isLoading"
          />
        </div>
        <div class="lingallery_delete_button">
          <button class="button-delete" @click="deleteSelectedPhoto(currentIndex)">
            <svg fill="#000000" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 64 64" width="22px" height="22px"><path d="M 30 2 C 29 2 28.101563 2.5 27.5 3.300781 L 24.5 8 L 13 8 C 11.300781 8 10 9.300781 10 11 L 10 17 C 10 18.699219 11.300781 20 13 20 L 13.097656 20 L 16.597656 53.5 C 16.898438 56.101563 19 58 21.597656 58 L 46.402344 58 C 49 58 51.101563 56.101563 51.402344 53.5 L 54.902344 20 L 55 20 C 56.699219 20 58 18.699219 58 17 L 58 11 C 58 9.300781 56.699219 8 55 8 L 43.5 8 L 40.402344 3.300781 C 39.902344 2.5 38.902344 2 37.902344 2 Z M 30.097656 4 L 38 4 C 38.300781 4 38.601563 4.199219 38.800781 4.398438 L 41.097656 8 L 26.902344 8 L 29.199219 4.398438 C 29.398438 4.199219 29.699219 4 30.097656 4 Z M 13 10 L 55 10 C 55.601563 10 56 10.398438 56 11 L 56 17 C 56 17.601563 55.601563 18 55 18 L 13 18 C 12.398438 18 12 17.601563 12 17 L 12 11 C 12 10.398438 12.398438 10 13 10 Z M 16 12 C 15.398438 12 15 12.398438 15 13 L 15 15 C 15 15.601563 15.398438 16 16 16 C 16.601563 16 17 15.601563 17 15 L 17 13 C 17 12.398438 16.601563 12 16 12 Z M 21 12 C 20.398438 12 20 12.398438 20 13 L 20 15 C 20 15.601563 20.398438 16 21 16 C 21.601563 16 22 15.601563 22 15 L 22 13 C 22 12.398438 21.601563 12 21 12 Z M 26 12 C 25.398438 12 25 12.398438 25 13 L 25 15 C 25 15.601563 25.398438 16 26 16 C 26.601563 16 27 15.601563 27 15 L 27 13 C 27 12.398438 26.601563 12 26 12 Z M 31 12 C 30.398438 12 30 12.398438 30 13 L 30 15 C 30 15.601563 30.398438 16 31 16 C 31.601563 16 32 15.601563 32 15 L 32 13 C 32 12.398438 31.601563 12 31 12 Z M 37 12 C 36.398438 12 36 12.398438 36 13 L 36 15 C 36 15.601563 36.398438 16 37 16 C 37.601563 16 38 15.601563 38 15 L 38 13 C 38 12.398438 37.601563 12 37 12 Z M 42 12 C 41.398438 12 41 12.398438 41 13 L 41 15 C 41 15.601563 41.398438 16 42 16 C 42.601563 16 43 15.601563 43 15 L 43 13 C 43 12.398438 42.601563 12 42 12 Z M 47 12 C 46.398438 12 46 12.398438 46 13 L 46 15 C 46 15.601563 46.398438 16 47 16 C 47.601563 16 48 15.601563 48 15 L 48 13 C 48 12.398438 47.601563 12 47 12 Z M 52 12 C 51.398438 12 51 12.398438 51 13 L 51 15 C 51 15.601563 51.398438 16 52 16 C 52.601563 16 53 15.601563 53 15 L 53 13 C 53 12.398438 52.601563 12 52 12 Z M 15.097656 20 L 52.902344 20 L 49.402344 53.300781 C 49.199219 54.800781 48 56 46.402344 56 L 21.597656 56 C 20.097656 56 18.800781 54.800781 18.597656 53.300781 Z M 34 25 C 33.398438 25 33 25.398438 33 26 L 33 46 C 33 46.601563 33.398438 47 34 47 C 34.601563 47 35 46.601563 35 46 L 35 26 C 35 25.398438 34.601563 25 34 25 Z M 25 25.097656 C 24.398438 25.097656 24 25.597656 24.097656 26.097656 L 25.097656 46.097656 C 25 46.597656 25.5 47 26 47 C 26.601563 47 27 46.5 27 46 L 26 26 C 26 25.398438 25.5 25 25 25.097656 Z M 43.097656 25.097656 C 42.5 25.097656 42.097656 25.5 42.097656 26 L 41.097656 46 C 41 46.5 41.398438 47 42 47 C 42.601563 47 43 46.597656 43 46.097656 L 44 26.097656 C 44 25.5 43.597656 25.097656 43.097656 25.097656 Z M 23 52 C 22.398438 52 22 52.398438 22 53 C 22 53.601563 22.398438 54 23 54 L 37 54 C 37.601563 54 38 53.601563 38 53 C 38 52.398438 37.601563 52 37 52 Z M 41 52 C 40.398438 52 40 52.398438 40 53 C 40 53.601563 40.398438 54 41 54 L 45 54 C 45.601563 54 46 53.601563 46 53 C 46 52.398438 45.601563 52 45 52 Z"/></svg>
          </button>
        </div>
        <picture-element
          v-if="addons.enablePictureElement"
          :alt="currentAlt"
          :is-loading="isLoading"
          :main-image-style="mainImageStyle"
          :items="items"
          :current-index="currentIndex"
          ref="mainImage"
          @handle-large-image-click="handleLargeImageClick"
          @handle-image-swipe="handleImageSwipe"
          @handle-image-loaded="handleImageLoaded"
        />
        <img
          v-else
          :alt="currentAlt"
          :class="{ loading: isLoading }"
          :src="currentImage"
          :style="mainImageStyle"
          @click="handleLargeImageClick"
          @load="handleImageLoaded"
          ref="mainImage"
          v-swipe="handleImageSwipe"
        />
        <div
          :style="captionStyle"
          class="lingallery_caption"
          v-if="currentCaption"
        >
          {{ currentCaption }}
        </div>
        <template v-if="showControls && items.length > 1">
          <a
            @click="showPreviousImage"
            class="control left"
            v-if="!leftControlClass"
            ><span style="position:relative;top:calc(50% - 12px)"
              >&#9664;</span
            ></a
          >
          <a
            @click="showNextImage"
            class="control right"
            v-if="!rightControlClass"
            ><span style="position:relative;top:calc(50% - 12px)"
              >&#9654;</span
            ></a
          >
          <a
            :class="'control left ' + leftControlClass"
            @click="showPreviousImage"
            v-if="leftControlClass"
          ></a>
          <a
            :class="'control right ' + rightControlClass"
            @click="showNextImage"
            v-if="rightControlClass"
          ></a>
        </template>
      </figure>
      <div class="lingallery_thumbnails" v-if="showThumbnails">
        <div class="lingallery_thumbnails_content">
          <div
            :key="index"
            class="lingallery_thumbnails_content_elem"
            v-for="(item, index) in items"
          >
            <img
              :alt="Object.prototype.hasOwnProperty.call(item, 'alt') ? item.alt : ''"
              :src="item.thumbnail"
              :style="thumbnailStyle(index)"
              height="64"
              v-on="
                currentIndex !== index
                  ? { click: () => handleImageClick(index) }
                  : {}
              "
            />
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { HalfCircleSpinner } from 'epic-spinners'
import Hammer from 'hammerjs'
import Addons from './addons/index.js'

export default {
  name: 'Lingallery',
  mixins: [Addons],
  directives: {
    swipe: {
      bind: function(el, binding) {
        if (typeof binding.value === 'function') {
          const hammerjs = new Hammer(el)
          hammerjs.get('swipe').set({ direction: Hammer.DIRECTION_HORIZONTAL, threshold: 5 })
          hammerjs.on('swipe', binding.value)
        }
      }
    }
  },
  data() {
    return {
      currentImage: null,
      currentImageWidth: 0,
      currentImageHeight: 0,
      currentIndex: 0,
      currentId: null,
      currentCaption: '',
      currentAlt: '',
      windowWidth: 0,
      isLoading: true,
      showLargeView: false,
      showGalleryLargeView: false,
    }
  },
  props: {
    items: {
      type: Array,
      default() {
        return []
      }
    },
    startImage: {
      type: Number,
      default: 0
    },
    width: {
      type: Number,
      default: 600
    },
    height: {
      type: Number,
      default: 400
    },
    baseColor: {
      type: String,
      default: '#fff'
    },
    accentColor: {
      type: String,
      default: '#3498db'
    },
    textColor: {
      type: String,
      default: '#000'
    },
    responsive: {
      type: Boolean,
      default: false
    },
    showThumbnails: {
      type: Boolean,
      default: true
    },
    mobileHeight: {
      type: Number,
      default: 0
    },
    mobileHeightBreakpoint: {
      type: Number,
      default: 0
    },
    leftControlClass: {
      type: String,
      default: ''
    },
    rightControlClass: {
      type: String,
      default: ''
    },
    disableImageClick: {
      type: Boolean,
      default: false
    },
    showControls: {
      type: Boolean,
      default: true
    },
    addons: {
      type: Object,
      default() {
        return {}
      }
    }
  },
  components: {
    HalfCircleSpinner
  },
  computed: {
    lingalleryStyle() {
      return this.windowWidth > this.width && !this.responsive
        ? 'width:' + this.width + 'px'
        : 'width:100%'
    },
    captionStyle() {
      return 'color:' + this.textColor
    },
    mainImageStyle() {
      let isLoading =
        this.$refs.mainImage &&
        !this.addons.enablePictureElement &&
        Object.prototype.hasOwnProperty.call(this.$refs.mainImage, 'classList') &&
        this.$refs.mainImage.classList.contains('loading')
          ? true
          : this.$refs.mainImage && this.addons.enablePictureElement
          ? this.$refs.mainImage.$el.children[
              this.$refs.mainImage.$el.children.length - 1
            ].classList.contains('loading')
          : false
      let mainImageStyle = ''

      if (
        this.mobileHeight !== 0 &&
        this.windowWidth < this.mobileHeightBreakpoint
      ) {
        mainImageStyle +=
          'width:100%;height:' + this.mobileHeight + 'px;object-fit:cover;'
      }
      if (isLoading) {
        if (this.mobileHeight !== 0) {
          mainImageStyle += 'min-height:' + this.mobileHeight + 'px;'
        } else mainImageStyle += 'min-height:200px;'
      }

      return mainImageStyle
    },
    figureStyle() {
      if (
        this.currentImage &&
        this.currentImageWidth &&
        this.currentImageHeight
      ) {
        let heightValue = 'height:auto'

        if (
          this.currentImageWidth < this.currentImageHeight &&
          this.mobileHeight === 0
        ) {
          heightValue = 'height:' + this.height + 'px'
        }
        return this.windowWidth > this.width && !this.responsive
          ? 'width:' + this.width + 'px;height:' + this.height + 'px'
          : 'width:100%;' + heightValue
      } else
        return this.windowWidth > this.width && !this.responsive
          ? 'width:' + this.width + 'px;height:' + this.height + 'px'
          : 'width:100%;height:auto'
    }
  },
  methods: {
    initLingallery() {
      this.currentImage = this.items[this.startImage].src
      this.currentCaption = this.items[this.startImage].caption
      this.currentId = this.items[this.startImage].id
      this.currentIndex = this.startImage
      this.windowWidth = window.innerWidth
      this.sendId()
    },
    refreshGallery(index) {
      this.currentImage = this.items[index].src
      this.currentCaption = this.items[index].caption
      this.currentId = this.items[index].id
      this.currentIndex = index
      this.windowWidth = window.innerWidth
    },
    handleImageLoaded() {
      this.isLoading = false
      this.updateCurrentImageSizes()
    },
    updateCurrentImageSizes() {
      let img =
        this.$refs.mainImage && Object.prototype.hasOwnProperty.call(this.$refs.mainImage, 'src')
          ? this.$refs.mainImage
          : this.$refs.mainImage.$el
          ? this.$refs.mainImage.$el.getElementsByTagName('img')[0]
          : null
      if (img) {
        this.currentImageWidth = img.naturalWidth
        this.currentImageHeight = img.naturalHeight
      }
    },
    handleImageSwipe(event) {
      if (event.direction === 4) {
        this.showPreviousImage()
      } else if (event.direction === 2) {
        this.showNextImage()
      }
    },
    handleImageClick(index) {
      this.currentIndex = index
      this.pickImage(index)
    },
    handleLargeImageClick() {
      if (this.addons.enableLargeView) {
        this.showLargeView = true
        return;
      }

      if (this.addons.enableGalleryLargeView) {
        this.showGalleryLargeView = true
        return;
      }

      if (!this.disableImageClick && this.items.length > 1) {
        this.showNextImage()
        return;
      }
    },
    handleLoader(isLoading) {
      this.isLoading = isLoading
    },
    pickImage(index) {
      // Show Loader
      this.handleLoader(true)

      this.currentImage = this.items[index].src
      this.currentCaption = Object.prototype.hasOwnProperty.call(this.items[index], 'caption')
        ? this.items[index].caption
        : ''
      this.currentAlt = Object.prototype.hasOwnProperty.call(this.items[index], 'alt')
        ? this.items[index].alt
        : ''
      this.currentId = Object.prototype.hasOwnProperty.call(this.items[index], 'id')
        ? this.items[index].id
        : null

      this.sendId()
    },
    thumbnailStyle(index) {
      let color =
        this.currentIndex === index ? this.accentColor : this.baseColor
      return 'border-color:' + color
    },
    sendId() {
      // This throws an uncomprehensible error so I commented it out for now
      // this.$emit('update:iid', this.currentId)
    },
    showNextImage() {
      // Show Loader
      this.handleLoader(true)

      if (this.items.length > this.currentIndex + 1) {
        this.currentIndex = this.currentIndex + 1
      } else {
        this.currentIndex = 0
      }

      this.pickImage(this.currentIndex)
    },
    showPreviousImage() {
      // Show Loader
      this.handleLoader(true)

      if (this.currentIndex !== 0) {
        this.currentIndex = this.currentIndex - 1
      } else {
        this.currentIndex = this.items.length - 1
      }

      this.pickImage(this.currentIndex)
    },
    deleteSelectedPhotos(data) {
      this.$emit('delete-images', data);
      this.deleteSelectedItems(data);
    },
    deleteSelectedPhoto(index) {
      const toBeDeleted = [];
      toBeDeleted.push(this.items[this.currentIndex]);

      this.$emit('delete-images', toBeDeleted);

      this.deleteSelectedItems(toBeDeleted);
    },
    deleteSelectedItems(items) {
      let lastIndex = 0;
      items.forEach(element => {
        let index = this.items.findIndex((item) => item.id === element.id);
        lastIndex = index;
        this.items.splice(index, 1);
      });

      this.refreshGallery(lastIndex+1);
    },
  },
  created() {
    this.initAddons()
  },
  mounted() {
    this.initLingallery()
  },
  watch: {
    items() {
      this.currentImage = this.items[this.startImage].src
    }
  }
}
</script>

<style lang="scss" scoped>
.lingalleryContainer {
  /deep/ .lingallery {
    max-width: 100%;
    figure {
      position: relative;
      margin: 0;
      padding: 0;
      max-width: 100%;
      text-align: center;
      cursor: pointer;
      img {
        max-width: 100%;
        max-height: 100%;
        transition: opacity 0.25s ease;
        &.loading {
          opacity: 0.25;
          min-height: 200px;
          transition: opcacity 0.25s ease;
        }
      }
      a.control {
        position: absolute;
        top: 0;
        padding-left: 5px;
        padding-right: 15px;
        height: 100%;
        display: none;
        font-size: 20px;
        color: #fff;
        cursor: pointer;
        text-shadow: 0 0 20px rgba(0, 0, 0, 0.75);
        &:before {
          position: relative;
          top: calc(50% - 12px);
        }
        &.left {
          left: 0;
        }
        &.right {
          right: 0;
        }
      }
      &:hover {
        a.control {
          display: block;
        }
      }
    }
    .lingallery_caption {
      position: absolute;
      bottom: 0;
      left: 0;
      padding: 4px 0;
      width: 100%;
      background-color: rgba(255, 255, 255, 0.75);
      font-size: 1em;
    }
    .lingallery_thumbnails {
      overflow-x: auto;
      width: 100%;
      scroll-snap-type: x mandatory;
      &::-webkit-scrollbar {
        height: 6px;
      }
      &::-webkit-scrollbar-track {
        border-radius: 10px;
        background: #eaeaea;
      }
      &::-webkit-scrollbar-thumb {
        border-radius: 10px;
        background: #b4b4b4;
      }
      &::-webkit-scrollbar-thumb:window-inactive {
        background: rgba(100, 100, 100, 0.4);
      }
      .lingallery_thumbnails_content {
        margin-top: 2px;
        width: auto;
        white-space: nowrap;
        .lingallery_thumbnails_content_elem {
          display: inline-block;
          scroll-snap-align: start;
          img {
            border: 2px solid #fff;
            cursor: pointer;
          }
        }
      }
    }
    .lingallery_spinner {
      position: absolute;
      left: 50%;
      top: calc(50% - 32px);
      > div {
        z-index: 9999;
        position: relative;
        left: -50%;
      }
    }
    .lingallery_delete_button {
      position: absolute;
      right: 0;

      .button-delete {
        width: 32px;
        height: 32px;
        border-radius: 50%;
        background: #a90329;
        border: none;

        position: relative;
        z-index: 50;
        top: -14px;
        right: -14px;

        svg {
          position: absolute;
          top: 50%;
          left: 50%;
          transform: translate(-50%, -50%);
          -ms-transform: translate(-50%, -50%);
          fill: #fff;
        }

        &:hover {
          background: #77021d;
        }

      }
    }
  }
}
</style>
