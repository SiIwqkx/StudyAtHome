<template>
  <div>
    <editor-field
      v-model="tempFormData.videoUrl"
      title="参考動画URL"
      label="video"
      placeholder="https://"
    />
    <video-thumbnail
      v-if="tempFormData.videoTitle && tempFormData.videoThumbnailUrl"
      :caption="tempFormData.videoTitle"
      title="動画サムネイル・キャプション表示"
      :thumbnail-url="tempFormData.videoThumbnailUrl"
    />
  </div>
</template>

<script lang="ts">
import { Vue, Component, Prop, Emit, Watch } from 'vue-property-decorator'
import EditorField from '~/components/EditorField.vue'

import VideoThumbnail from '@/components/VideoThumbnail.vue'

export type formData = {
  videoUrl: string
  videoTitle: string
  videoThumbnailUrl: string
}
@Component({
  components: {
    EditorField,
    VideoThumbnail
  }
})
export default class EditingScreen3 extends Vue {
  tempFormData = {
    videoUrl: this.form.videoUrl,
    videoTitle: this.form.videoTitle,
    videoThumbnailUrl: this.form.videoThumbnailUrl
  }

  @Prop({
    type: Object as () => formData,
    required: true,
    default: () => ({
      videoUrl: '',
      videoTitle: '',
      videoThumbnailUrl: ''
    })
  })
  public value!: formData

  private get form(): formData {
    return this.value
  }

  @Watch('tempFormData', { deep: true })
  onChangeTempFormData() {
    this.input(this.tempFormData)
  }

  /* CORS 回避必須
  @Watch('form.videoUrl')
  onChangeVideoUrl() {
    fetch(this.form.videoUrl)
      .then(res => res.text())
      .then(text => {
        const el = new DOMParser().parseFromString(text, 'text/html')
        const headEls = el.head.children
        Array.from(headEls).map(v => {
          const prop = v.getAttribute('property')
          if (!prop) return
          if (prop === 'og:title') {
            this.form.videoTitle = '' + v.getAttribute('content')
          }
          if (prop === 'og:image') {
            this.form.videoThumbnailUrl = '' + v.getAttribute('content')
          }
          console.log(prop, v.getAttribute('content'))
        })
      })
  }
  */
  @Emit()
  public input(value: formData) {
    return value
  }
}
</script>
