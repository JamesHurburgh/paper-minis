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
                <v-slider label="Copies" v-model="copies" :min="1" :max="20" :step="1" thumb-label></v-slider>
                <v-expansion-panels>
                  <v-expansion-panel key="Image">
                    <v-expansion-panel-title>
                      Image
                    </v-expansion-panel-title>
                    <v-expansion-panel-text>
                      <v-row>
                        <v-col md="9">
                          <v-text-field v-model="imageUrl" label="Image URL"></v-text-field>
                        </v-col>
                        <v-col md="3">
                          <v-select label="Size" v-model="size" :items="sizes" :item-props="itemProps"></v-select>
                        </v-col>
                      </v-row>
                      <v-row>
                        <v-col md="9">
                          <v-slider label="Rounded Edges" v-model="imageRoundedEdgeAmount" :min="0"
                            :max="sizes.filter(s => s.value === size)[0]!.width / 2" :step="0.1" thumb-label></v-slider>
                        </v-col>
                        <v-col md="3">
                          <v-btn color="primary">Background Colour
                            <v-dialog v-model="imageBackgroundColourDialog" activator="parent" width="auto">
                              <v-card>
                                <v-card-title>
                                  <span class="text-h5">Select colour</span>
                                </v-card-title>
                                <v-card-text>
                                  <v-color-picker v-model="imageBackgroundColour" hide-inputs
                                    show-swatches></v-color-picker>
                                </v-card-text>
                                <v-card-actions>
                                  <v-btn color="primary" block @click="imageBackgroundColourDialog = false">Close
                                    Dialog</v-btn>
                                </v-card-actions>
                              </v-card>
                            </v-dialog>
                          </v-btn>
                        </v-col>
                      </v-row>
                    </v-expansion-panel-text>
                  </v-expansion-panel>
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
                      <v-row>
                        <v-col md="6">
                          <v-text-field v-model="name" label="Name"></v-text-field>
                        </v-col>
                        <v-col md="3">
                          <v-switch v-model="numbered" label="Numbered"></v-switch>
                        </v-col>
                        <v-col md="3">
                          <v-text-field v-if="numbered" v-model="startNumber" persistent-hint hint="Start Number"
                            single-line type="number" />
                        </v-col>
                        <!-- <v-col md="9">
                          <v-slider label="Rounded Edges" v-model="baseRoundedEdgeAmount" :min="0"
                            :max="sizes.filter(s => s.value === size)[0]!.width / 2" :step="0.1" thumb-label></v-slider>
                        </v-col> -->
                        <v-col md="3">
                          <v-btn color="primary">Background Colour
                            <v-dialog v-model="baseBackgroundColourDialog" activator="parent" width="auto">
                              <v-card>
                                <v-card-title>
                                  <span class="text-h5">Select base colour</span>
                                </v-card-title>
                                <v-card-text>
                                  <v-color-picker v-model="baseBackgroundColour" hide-inputs
                                    show-swatches></v-color-picker>
                                </v-card-text>
                                <v-card-actions>
                                  <v-btn color="primary" block @click="baseBackgroundColourDialog = false">Close
                                    Dialog</v-btn>
                                </v-card-actions>
                              </v-card>
                            </v-dialog>
                          </v-btn>
                        </v-col>
                      </v-row>
                    </v-expansion-panel-text>
                  </v-expansion-panel>
                  <v-expansion-panel key="Base">
                    <v-expansion-panel-title>
                      Page Setup
                    </v-expansion-panel-title>
                    <v-expansion-panel-text>
                      <v-select label="Page orientation" v-model="pageOrientation" :items="pageOrientations"></v-select>
                      <v-select label="Page size" v-model="pageSize" :items="pageSizes"
                        :item-props="pageSizeProp"></v-select>
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
                      <div v-if="numbered" class="rotated">{{ index + Number(startNumber) }}</div>
                      <div v-else>&nbsp;</div>
                      <div v-if="name" class="rotated">{{ name }}</div>
                      <div v-else>&nbsp;</div>
                    </div>
                    <v-img :style="imageStyle" :class="`${size} flip-vertical`" :src="`${imageUrl}`" />
                    <v-img :style="imageStyle" :class="`${size}`" :src="`${imageUrl}`" />
                    <div :style="`${tokenStyle} ${baseStyle}`" :class="`${size} ${size}-half-base`">
                      <div v-if="name">{{ name }}</div>
                      <div v-else>&nbsp;</div>
                      <div v-if="numbered">{{ index + Number(startNumber) }}</div>
                      <div v-else>&nbsp;</div>
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
<script lang="ts">
import { defineComponent } from 'vue'
import { usePaperizer } from 'paperizer'
const printableMinisCssPath = "./printableminis.css"
var cssId = printableMinisCssPath;  // you could encode the css path itself to generate id..
if (!document.getElementById(cssId)) {
  var head = document.getElementsByTagName('head')[0];
  var link = document.createElement('link');
  link.id = cssId;
  link.rel = 'stylesheet';
  link.type = 'text/css';
  link.href = printableMinisCssPath;
  link.media = 'all';
  head.appendChild(link);
}

export default defineComponent({
  name: 'PaperMiniMaker}',

  data: () => ({
    imageUrl: "https://i.imgur.com/bYJ3gBl.png",
    imageBackgroundColour: "lightblue",
    imageBackgroundColourDialog: false,
    imageRoundedEdgeAmount: 0.4,
    name: "Werewolf",
    copies: 14,
    size: "medium",
    borderStyle: "solid",
    borderWidth: 1,
    baseBackgroundType: "solid",
    baseBackgroundColour: "darkgrey",
    baseBackgroundColourDialog: false,
    baseRoundedEdgeAmount: 0.5,
    numbered: true,
    startNumber: 1,
    sizes: [
      {
        title: "Tiny",
        value: "tiny",
        subtitle: "1/2 inch",
        width: 0.5
      },
      {
        title: "Small/Medium",
        value: "medium",
        subtitle: "1 inch",
        width: 1
      },
      {
        title: "Large",
        value: "large",
        subtitle: "2 inches",
        width: 2
      },
      {
        title: "Huge",
        value: "huge",
        subtitle: "3 inches",
        width: 3
      },
      {
        title: "Gargantuan",
        value: "gargantuan",
        subtitle: "4 inches",
        width: 4
      }
    ],
    pageOrientations: [
      "portrait",
      "landscape"
    ],
    pageOrientation: "portrait",
    pageSize: "A4",
    pageSizes: [
      {
        name: "A4",
        value: "A4",
        width: "8.3",
        height: "11.7"
      },
      // {
      //   name: "A3",
      //   value: "A3",
      //   width: "11.7",
      //   height: "16.5"
      // },
      {
        name: "Letter",
        value: "Letter",
        width: "8.5",
        height: "11"
      }
    ]
  }),
  computed: {
    imageStyle() {
      return `border-style: ${this.borderStyle};` +
        `border-width: ${this.borderWidth}px;` +
        `border-top-left-radius: ${this.imageRoundedEdgeAmount}in;` +
        `border-top-right-radius: ${this.imageRoundedEdgeAmount}in;` +
        `background-color: ${this.imageBackgroundColour} !important; print-color-adjust: exact;`
    },
    tokenStyle() {
      return `border-style: ${this.borderStyle};` +
        `border-width: ${this.borderWidth}px;`
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
    pageSizeProp(item: any) {
      return {
        title: item.name,
        subtitle: this.pageOrientation == "portrait" ? `${item.width}" x ${item.height}"` : `${item.height}" x ${item.width}"`
      }
    },
    print() {
      const { paperize } = usePaperizer('print-me', {
        styles: [
          'https://cdn.jsdelivr.net/npm/vuetify@3.0.5/dist/vuetify.min.css',
          printableMinisCssPath,
          `./pages/${this.pageSize}.${this.pageOrientation}.css`
        ],
        autoClose: true
      })
      paperize()
    }
  },
})
</script>
