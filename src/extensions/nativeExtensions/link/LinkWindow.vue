<template>
  <v-dialog :value="value" max-width="500px">
    <v-card rounded="xl">
      <v-card-title>
        <span class="headline">
          {{ $i18n.getMsg('extensions.Link.window.title') }}
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
        <v-text-field
          v-model="form.href"
          filled
          rounded
          :label="$i18n.getMsg('extensions.Link.window.form.hrefLabel')"
        />
        <!--
        <v-checkbox
          v-model="form.blank"
          label="Is blank?"
        />
        -->
      </v-card-text>
      <v-card-actions>
        <v-row no-gutters justify="center" align="center">
          <v-spacer />
          <v-col cols="3">
            <v-btn
              :disabled="isDisabled"
              rounded
              class="blue white--text"
              width="100px"
              @click="apply"
            >
              {{ $i18n.getMsg('extensions.Link.window.buttons.apply') }}
            </v-btn>
          </v-col>
          <v-col cols="3">
            <v-btn rounded class="red white--text" width="100px" @click="close">
              {{ $i18n.getMsg('extensions.Link.window.buttons.close') }}
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
  VTextField
} from 'vuetify/lib'
import I18nMixin from '~/mixins/I18nMixin'
import { COMMON_ICONS } from '~/configs/theme'

export const PROPS = {
  VALUE: 'value' as const,
  CONTEXT: 'context' as const,
  EDITOR: 'editor' as const,
  NATIVE_EXTENSION_NAME: 'nativeExtensionName' as const,
  HREF: 'href' as const
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
    VTextField
  }
})
export default class LinkWindow extends mixins(I18nMixin) {
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
    type: String
  })
  readonly [PROPS.HREF]: null | string

  readonly COMMON_ICONS = COMMON_ICONS

  form = {
    href: this[PROPS.HREF],
    blank: false
  }

  get isRemove() {
    return this[PROPS.HREF] && !this.form.href
  }

  get isDisabled() {
    return (
      this[PROPS.HREF] === this.form.href ||
      (!this[PROPS.HREF] && !this.form.href)
    )
  }

  apply() {
    this[PROPS.CONTEXT].commands[this[PROPS.NATIVE_EXTENSION_NAME]]({
      href: this.form.href
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
