<template>
  <v-dialog :value="value" max-width="500px">
    <v-card rounded="xl">
      <v-card-title>
        <span class="headline">
          表
        </span>
        <!-- <v-spacer />
        <v-btn
          icon
          @click="close"
        >
          <v-icon>{{ COMMON_ICONS.close[$tiptapVuetify.iconsGroup] }}</v-icon>
        </v-btn> -->
      </v-card-title>
      <v-card-text>
        <v-row no-gutters justify="center" align="center">
          <v-col cols="6" class="text-center">
            <v-text-field v-model="form.rowsCount" filled label="行数" />
          </v-col>
          <v-col cols="6" class="text-center">
            <v-text-field v-model="form.colsCount" filled label="列数" />
          </v-col>
        </v-row>
        <v-row no-gutters justify="center" align="center">
          <v-spacer />
          <v-col cols="4" class="text-center">
            <v-checkbox v-model="form.withHeaderRow" label="ヘッダあり" />
          </v-col>
        </v-row>
      </v-card-text>
      <v-card-actions>
        <v-row no-gutters justify="center" align="center">
          <v-spacer />
          <v-col cols="3">
            <v-btn
              rounded
              class="blue white--text"
              width="100px"
              @click="apply"
            >
              確定
            </v-btn>
          </v-col>
          <v-col cols="3">
            <v-btn rounded class="red white--text" width="100px" @click="close">
              閉じる
            </v-btn>
          </v-col>
          <v-spacer />
        </v-row>
      </v-card-actions>
    </v-card>
  </v-dialog>
</template>

<script lang="ts">
import { mixins } from 'vue-class-component'
import { Component, Prop } from 'vue-property-decorator'
import {
  VRow,
  VCol,
  VDialog,
  VCard,
  VCardTitle,
  VCardText,
  VCardActions,
  VBtn,
  VSpacer,
  VIcon,
  VTextField,
  VCheckbox
} from 'vuetify/lib'
import I18nMixin from '~/mixins/I18nMixin'
import { COMMON_ICONS } from '~/configs/theme'

export const PROPS = {
  VALUE: 'value' as const,
  CONTEXT: 'context' as const,
  EDITOR: 'editor' as const,
  IMAGE_SOURCES: 'imageSources' as const,
  IMAGE_SOURCES_OVERRIDE: 'imageSourcesOverride' as const,
  NATIVE_EXTENSION_NAME: 'nativeExtensionName' as const
}

@Component({
  components: {
    VRow,
    VCol,
    VDialog,
    VCard,
    VCardTitle,
    VCardText,
    VCardActions,
    VBtn,
    VSpacer,
    VIcon,
    VTextField,
    VCheckbox
  }
})
export default class TableWindow extends mixins(I18nMixin) {
  @Prop({
    type: Boolean,
    default: false
  })
  readonly [PROPS.VALUE]: boolean;

  @Prop({
    type: String,
    required: true
  })
  readonly [PROPS.NATIVE_EXTENSION_NAME]: string;

  @Prop({
    type: Object,
    required: true
  })
  readonly [PROPS.CONTEXT]: any;

  @Prop({
    type: Object,
    required: true
  })
  readonly [PROPS.EDITOR]: any;

  @Prop({
    type: Boolean,
    required: false
  })
  readonly [PROPS.IMAGE_SOURCES_OVERRIDE]: any

  readonly COMMON_ICONS = COMMON_ICONS

  form = {
    rowsCount: 2,
    colsCount: 3,
    withHeaderRow: false
  }

  apply() {
    this[PROPS.CONTEXT].commands.createTable({
      rowsCount: this.form.rowsCount,
      colsCount: this.form.colsCount,
      withHeaderRow: this.form.withHeaderRow
    })
    this.close()
    this[PROPS.EDITOR].focus()
  }

  close() {
    this.$destroy()
    this.$el.parentNode!.removeChild(this.$el)
  }
}
</script>
