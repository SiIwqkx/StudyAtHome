<template>
  <div>
    <editor-field
      v-model="tempFormData.pages"
      title="教科書ページ"
      label="textbook_page"
      placeholder="例）10〜14ページ"
    />
    <editor-field
      v-model="tempFormData.materialsTitle"
      title="副教材タイトル"
      label="sub_text"
      placeholder="例）やさしい理科教材"
    />
    <editor-field
      v-model="tempFormData.materialsUrl"
      title="副教材URL"
      label="sub_text_url"
      placeholder="https://"
    />
  </div>
</template>

<script lang="ts">
import { Vue, Component, Prop, Emit, Watch } from 'vue-property-decorator'
import EditorField from '~/components/EditorField.vue'

export type formData = {
  pages: string
  materialsTitle: string
  materialsUrl: string
}
@Component({
  components: {
    EditorField
  }
})
export default class EditingScreen1 extends Vue {
  tempFormData = {
    pages: this.form.pages,
    materialsTitle: this.form.materialsTitle,
    materialsUrl: this.form.materialsUrl
  }

  @Prop({
    type: Object as () => formData,
    required: true,
    default: () => ({
      pages: '',
      materialsTitle: '',
      materialsUrl: ''
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

  @Emit()
  public input(value: formData) {
    return value
  }
}
</script>
