<template>
  <v-container class="fill-height">
    <v-responsive class="fill-height">
      <v-container fluid>
        <v-row>
          <v-col md="6" class="no-print">
            <v-card prepend-icon="mdi-cog">
              <template v-slot:title>
                Settings
              </template>
              <template v-slot:append>
                <v-btn prepend-icon="mdi-printer" @click="print">Print</v-btn>
              </template>
              <v-card-subtitle>
                Adjust values here to change what the paper minis will look like.
              </v-card-subtitle>
              <v-card-text>
                <h3>Image</h3>
                <v-row>
                  <v-col md="9">
                    <v-text-field v-model="imageUrl" label="Image URL"></v-text-field>
                  </v-col>
                  <v-col md="3">
                    <v-select label="Size" v-model="size" :items="sizes" :item-props="itemProps"></v-select>
                  </v-col>
                </v-row>
                <h3>Text</h3>
                <v-text-field v-model="name" label="Name"></v-text-field>
                <v-switch v-model="numbered" label="Numbered" hide-details></v-switch>
                <v-slider label="Copies" v-model="copies" :min="1" :max="20" :step="1" thumb-label></v-slider>
                <v-expansion-panels>
                  <v-expansion-panel key="Border">
                    <v-expansion-panel-title>
                      Border
                    </v-expansion-panel-title>
                    <v-expansion-panel-text>
                      <p>Style: {{ borderStyle }}</p>
                      <v-radio-group v-model="borderStyle">
                        <v-radio label="solid" value="solid"></v-radio>
                        <v-radio label="dashed" value="dashed"></v-radio>
                        <v-radio label="dotted" value="dotted"></v-radio>
                      </v-radio-group>
                      <v-slider label="Width" v-model="borderWidth" :min="0" :max="5" :step="1" thumb-label></v-slider>
                    </v-expansion-panel-text>
                  </v-expansion-panel>
                  <v-expansion-panel key="Base">
                    <v-expansion-panel-title>
                      Base
                    </v-expansion-panel-title>
                    <v-expansion-panel-text>
                      <v-color-picker v-model="baseBackgroundColour" hide-inputs show-swatches></v-color-picker>
                    </v-expansion-panel-text>
                  </v-expansion-panel>
                </v-expansion-panels>
              </v-card-text>
            </v-card>
          </v-col>
          <v-col md="6" class="justify-center">
            <v-card flat>
              <template v-slot:title>
                <span class="no-print">
                  <v-icon icon="mdi-image" size="small"></v-icon>
                  Preview
                </span>
              </template>
              <v-card-subtitle class="no-print">
                This preview shows what the paper minis may look like.
              </v-card-subtitle>

              <v-card-text>
                <div id="print-me">
                  <div v-for="(item, index) in copies" :key="index" style="float: left;" class="align-center text-center">
                    <div :style="`${tokenStyle} ${baseStyle}`" :class="`${size} ${size}-half-base`">
                      <div v-if="numbered" class="rotated">{{ index + 1 }}</div><div v-else>&nbsp;</div>
                      <div v-if="name" class="rotated">{{ name }}</div><div v-else>&nbsp;</div>
                    </div>
                    <v-img :style="tokenStyle" :class="`${size} flip-vertical`" :src="`${imageUrl}`" />
                    <v-img :style="tokenStyle" :class="`${size}`" :src="`${imageUrl}`" />
                    <div :style="`${tokenStyle} ${baseStyle}`" :class="`${size} ${size}-half-base`">
                      <div v-if="name">{{ name }}</div><div v-else>&nbsp;</div>
                      <div v-if="numbered">{{ index + 1 }}</div><div v-else>&nbsp;</div>
                    </div>
                    <div :style="tokenStyle" :class="`${size} ${size}-base`"></div>
                  </div>
                </div>
              </v-card-text>
            </v-card>
          </v-col>
        </v-row>
      </v-container>
    </v-responsive>
  </v-container>
</template>
<style scoped>
@media print {

  body {
    -webkit-print-color-adjust: exact !important;
    print-color-adjust: exact !important;
  }

  .no-print,
  .no-print * {
    display: none !important;
  }

  .v-content {
    padding: 0 !important;
  }

  @page {
    /* size: landscape */
  }
}

.token {}

.tiny {
  width: 0.5in;
}

.tiny-half-base {
  height: 0.25in;
}

.tiny-base {
  height: 0.5in;
}

.small {
  width: 1in;
}

.small-half-base {
  height: 0.5in;
}

.small-base {
  height: 1in;
}

.medium {
  width: 1in;
}

.medium-half-base {
  height: 0.5in;
}

.medium-base {
  height: 1in;
}

.large {
  width: 2in;
}

.large-half-base {
  height: 1in;
}

.large-base {
  height: 2in;
}

.huge {
  width: 3in;
}

.huge-half-base {
  height: 1.5in;
}

.huge-base {
  height: 3in;
}

.gargantuan {
  width: 4in;
}

.gargantuan-half-base {
  height: 2in;
}

.gargantuan-base {
  height: 4in;
}

.flip-horizontal {
  -webkit-transform: scaleX(-1);
  transform: scaleX(-1);
}

.flip-vertical {
  -webkit-transform: scaleY(-1);
  transform: scaleY(-1);
}

.rotated {
  -webkit-transform: rotate(180deg);
  transform: rotate(180deg);
}
</style>
<script lang="ts">
import { defineComponent } from 'vue'
export default defineComponent({
  name: 'PaperMiniMaker}',

  data: () => ({
    imageUrl: "https://www.dndbeyond.com/avatars/thumbnails/30783/955/1000/1000/638062024584880857.png",
    name: "Goblin",
    copies: 14,
    size: "medium",
    borderStyle: "solid",
    borderWidth: 1,
    baseBackgroundType: "solid",
    baseBackgroundColour: "lightgrey",
    numbered: true,
    sizes: [
      {
        title: "Tiny",
        value: "tiny",
        subtitle: "1/2 inch"
      },
      {
        title: "Small/Medium",
        value: "medium",
        subtitle: "1 inch"
      },
      {
        title: "Large",
        value: "large",
        subtitle: "2 inches"
      },
      {
        title: "Huge",
        value: "huge",
        subtitle: "3 inches"
      },
      {
        title: "Gargantuan",
        value: "gargantuan",
        subtitle: "4 inches"
      }
    ]
  }),
  computed: {
    tokenStyle() {
      return `border-style: ${this.borderStyle}; border-width: ${this.borderWidth}px;`
    },
    baseStyle() {
      switch (this.baseBackgroundType) {
        case "solid":
          return `background-color: ${this.baseBackgroundColour} !important; print-color-adjust: exact;`
        default:
          return ""
      }
    }
  },
  methods: {
    itemProps(item: any) {
      return {
        title: item.title,
        subtitle: item.subtitle,
      }
    },
    print() {
      print()
      // this.$paperize('print-me', { styles: ["https://cdn.jsdelivr.net/npm/vuetify@2.x/dist/vuetify.min.css"] })
    }
  },
})
</script>
