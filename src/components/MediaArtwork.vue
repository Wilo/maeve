<template>
  <div class="artwork">
    <img
      class="artwork__image"
      v-lazy="artworkUrlForArtist || artworkUrl"
      alt
      :style="artworkImageStyle"
    />
  </div>
</template>

<script lang="ts">
import { Component, Prop, Vue } from 'vue-property-decorator';
import { getArtworkUrl } from '@/utils/utils';

@Component
export default class MediaArtwork extends Vue {
  // Only artists need to pass in artwork url as Artwork is not available under artists for now.
  // For other types (albums, playlists, tracks...) pass the artwork object instead
  @Prop()
  artworkUrlForArtist!: string;
  @Prop()
  artwork!: MusicKit.Artwork;
  @Prop()
  width!: number;
  @Prop()
  height!: number;
  @Prop({ default: false })
  hasShadow!: boolean;
  @Prop({ default: false })
  isRound!: boolean;

  get artworkImageStyle(): object {
    const style: any = {};
    if (this.isRound) {
      style['border-radius'] = '50%';
    }

    if (this.hasShadow) {
      style['box-shadow'] = `0.2rem 0.2rem 1rem #${this.artwork.textColor1}`;
    }

    return style;
  }

  get artworkUrl(): string {
    if (!this.artwork) {
      return '';
    }
    return getArtworkUrl(this.artwork.url, this.width, this.height);
  }
}
</script>

<style scoped lang="scss">
.artwork {
  // height: 100%;
  position: relative;
  // overflow: hidden;
  width: 100%;
}

.artwork__image-wrapper {
  align-items: center;
  display: flex;
  height: 100%;
  width: 100%;
}

.artwork__image {
  display: block;
  height: 100%;
  width: 100%;
}
</style>
