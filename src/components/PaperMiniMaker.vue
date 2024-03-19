<template>
  <v-container class="fill-height" :class="{'pa-0 ma-0': $vuetify.display.smAndDown}">
    <v-responsive class="fill-height">
      <v-container fluid :class="{'pa-0 ma-0': $vuetify.display.smAndDown}">
        <v-row>
          <v-col md="6">
            <v-card prepend-icon="mdi-cog">
              <template v-slot:title>
                Settings
              </template>
              <template v-slot:append>
                <v-btn prepend-icon="mdi-shuffle" @click="randomiseStyle">
                  <div class="d-none d-lg-block">Random</div>
                </v-btn>
                <v-btn prepend-icon="mdi-restart" @click="resetStyle">
                  <div class="d-none d-lg-block">Reset</div>
                </v-btn>
                <v-btn prepend-icon="mdi-printer" @click="print">
                  <div class="d-none d-lg-block">Print</div>
                </v-btn>
              </template>
              <v-card-subtitle>
                Token style and layout.
              </v-card-subtitle>
              <v-card-text :class="{'pa-0 ma-0': $vuetify.display.smAndDown}">
                <v-expansion-panels variant="accordion">
                  <v-expansion-panel key="Layout">
                    <v-expansion-panel-title>
                      Sheet
                    </v-expansion-panel-title>
                    <v-expansion-panel-text>
                      <v-row>
                        <v-col md="12">
                          <div class="text-caption">Number of tokens</div>
                          <v-slider md="9" v-model="sheet.copies" :min="1" :max="20" :step="1"
                            thumb-label></v-slider>
                        </v-col>
                      </v-row>
                      <v-row>
                        <v-col md="6">
                          <div class="text-caption">Margins</div>
                          <v-slider v-model="sheet.margins" :min="0" :max="0.5" :step="0.05"
                            thumb-label></v-slider>
                        </v-col>
                        <v-col md="3">
                          <div class="text-caption">Numbered</div>
                          <v-switch density="compact" v-model="sheet.numbered"></v-switch>
                        </v-col>
                        <v-col md="3">
                          <v-text-field density="compact" v-if="sheet.numbered" v-model="sheet.startNumber"
                            persistent-hint hint="Start Number" single-line type="number" />
                        </v-col>
                      </v-row>

                    </v-expansion-panel-text>
                  </v-expansion-panel>
                  <v-expansion-panel key="Image">
                    <v-expansion-panel-title>
                      Token
                    </v-expansion-panel-title>
                    <v-expansion-panel-text>
                      <v-row>
                        <v-col md="5">
                          <v-text-field v-model="token.imageUrl" label="Image URL"></v-text-field>
                        </v-col>
                        <v-col md="5">
                          <v-text-field v-model="token.reverseImageUrl" label="Reverse Image URL"></v-text-field>
                        </v-col>
                        <v-col md="2" v-if="token.reverseImageUrl">
                          <div class="text-caption">Flip Reverse Image</div>
                          <v-switch density="compact" v-model="token.flipReverseImage"></v-switch>
                        </v-col>
                        <v-col md="6">
                          <v-text-field v-model="token.name" label="Name" clearable></v-text-field>
                        </v-col>
                        <v-col md="3">
                          <v-select label="Size" v-model="token.size" :items="sizes" :item-props="itemProps"></v-select>
                        </v-col>
                        <v-col md="6">
                          <v-slider label="Rounded Edges" v-model="style.imageRoundedEdgeAmount" :min="0" :max="100"
                            :step="1" thumb-label></v-slider>
                        </v-col>
                        <v-col md="3">
                          <v-btn color="primary" append-icon="mdi-palette">
                            <div class="d-none d-lg-block">Background</div>
                            <v-dialog v-model="imageBackgroundColourDialog" activator="parent" width="auto">
                              <v-card>
                                <v-card-title>
                                  <span class="text-h5">Select colour</span>
                                </v-card-title>
                                <v-card-text>
                                  <v-color-picker v-model="style.imageBackgroundColour" hide-inputs show-swatches
                                    :swatches="swatches"></v-color-picker>
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
                      Style
                    </v-expansion-panel-title>
                    <v-expansion-panel-text>
                      <v-radio-group label="Border Style" v-model="style.borderStyle">
                        <v-radio label="solid" value="solid"></v-radio>
                        <v-radio label="dashed" value="dashed"></v-radio>
                        <v-radio label="dotted" value="dotted"></v-radio>
                      </v-radio-group>
                      <v-row>

                        <v-col md="9">
                          <v-slider label="Border Width" v-model="style.borderWidth" :min="0" :max="5" :step="1"
                            thumb-label></v-slider>
                        </v-col>
                        <v-col md="3">
                          <v-btn color="primary" append-icon="mdi-palette">
                            <div class="d-none d-lg-block">Border</div>
                            <v-dialog v-model="borderColourDialog" activator="parent" width="auto">
                              <v-card>
                                <v-card-title>
                                  <span class="text-h5">Select colour</span>
                                </v-card-title>
                                <v-card-text>
                                  <v-color-picker v-model="style.borderColour" show-swatches
                                    :swatches="swatches"></v-color-picker>
                                </v-card-text>
                                <v-card-actions>
                                  <v-btn color="primary" block @click="borderColourDialog = false">Close
                                    Dialog</v-btn>
                                </v-card-actions>
                              </v-card>
                            </v-dialog>
                          </v-btn>
                        </v-col>
                        <v-col md="9">
                          <v-slider label="Rounded Edges" v-model="style.baseRoundedEdgeAmount" :min="0" :max="100"
                            :step="1" thumb-label></v-slider>
                        </v-col>
                        <v-col md="3">
                          <v-btn color="primary" append-icon="mdi-palette">
                            <div class="d-none d-lg-block">Background</div>
                            <v-dialog v-model="baseBackgroundColourDialog" activator="parent" width="auto">
                              <v-card>
                                <v-card-title>
                                  <span class="text-h5">Select base colour</span>
                                </v-card-title>
                                <v-card-text>
                                  <v-color-picker v-model="style.baseBackgroundColour" show-swatches
                                    :swatches="swatches"></v-color-picker>
                                </v-card-text>
                                <v-card-actions>
                                  <v-btn color="primary" block @click="baseBackgroundColourDialog = false">Close
                                    Dialog</v-btn>
                                </v-card-actions>
                              </v-card>
                            </v-dialog>
                          </v-btn>
                        </v-col>
                        <v-col md="6">
                          <v-select label="Font Family" v-model="style.fontFamily" :items="fontFamilies"
                            item-value="value" return-object>
                            <template v-slot:item="{ props, item }">
                              <v-list-item v-bind="props" :style="`font-family: ${item.value};`" :title="item.title">
                              </v-list-item>
                            </template>
                          </v-select>
                        </v-col>
                        <v-col md="3">
                          <v-select v-model="style.fontSize" label="Font Size"
                            :items="Array.from({ length: 30 }, (v, i) => i)"></v-select>
                        </v-col>
                        <v-col md="3">
                          <v-btn color="primary" append-icon="mdi-palette">
                            <div>Text</div>
                            <v-dialog v-model="baseTextColourDialog" activator="parent" width="auto">
                              <v-card>
                                <v-card-title>
                                  <span class="text-h5">Select text colour</span>
                                </v-card-title>
                                <v-card-text>
                                  <v-color-picker v-model="style.baseTextColour" show-swatches
                                    :swatches="swatches"></v-color-picker>
                                </v-card-text>
                                <v-card-actions>
                                  <v-btn color="primary" block @click="baseTextColourDialog = false">Close
                                    Dialog</v-btn>
                                </v-card-actions>
                              </v-card>
                            </v-dialog>
                          </v-btn>
                        </v-col>
                        <v-col md="3">
                          <v-select v-model="style.baseTextOutlineWidth" label="Outline Width"
                            :items="Array.from({ length: 3 }, (v, i) => i)"></v-select>
                        </v-col>
                        <v-col md="3">
                          <v-btn color="primary" append-icon="mdi-palette">
                            <div>Text Outline Colour</div>
                            <v-dialog v-model="baseTextOutlineColourDialog" activator="parent" width="auto">
                              <v-card>
                                <v-card-title>
                                  <span class="text-h5">Select text outline colour</span>
                                </v-card-title>
                                <v-card-text>
                                  <v-color-picker v-model="style.baseTextOutlineColour" show-swatches
                                    :swatches="swatches"></v-color-picker>
                                </v-card-text>
                                <v-card-actions>
                                  <v-btn color="primary" block @click="baseTextOutlineColourDialog = false">Close
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
                The paper minis may look like this.
              </v-card-subtitle>

              <v-card-text>
                <div id="print-me">
                  <div v-for="(item, index) in sheet.copies" :key="index" :style="`float: left; ${fullTokenStyle}`"
                    class="align-center text-center">
                    <div :style="`${tokenStyle} ${baseStyle}`" :class="`rotated ${token.size} ${token.size}-half-base`">
                      <div v-if="token.name">{{ token.name }}</div>
                      <div v-else>&nbsp;</div>
                      <div v-if="sheet.numbered">{{ index + Number(sheet.startNumber) }}</div>
                      <div v-else>&nbsp;</div>
                    </div>
                    <v-img v-if="token.reverseImageUrl" :style="imageStyle" :class="`${token.size} ${token.flipReverseImage ? 'flip-horizontal-and-vertical' : 'flip-vertical'}`" :src="`${token.reverseImageUrl}`" />
                    <v-img v-else :style="imageStyle" :class="`${token.size} flip-vertical`" :src="`${token.imageUrl}`" />
                    <v-img :style="imageStyle" :class="`${token.size}`" :src="`${token.imageUrl}`" />
                    <div :style="`${tokenStyle} ${baseStyle}`" :class="`${token.size} ${token.size}-half-base`">
                      <div v-if="token.name">{{ token.name }}</div>
                      <div v-else>&nbsp;</div>
                      <div v-if="sheet.numbered">{{ index + Number(sheet.startNumber) }}</div>
                      <div v-else>&nbsp;</div>
                    </div>
                    <div :style="tokenStyle" :class="`${token.size} ${token.size}-base`"></div>
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
    token: {
      imageUrl: "https://i.imgur.com/bYJ3gBl.png",
      reverseImageUrl: null,
      flipReverseImage: false,
      name: "Werewolf",
      size: "medium",
    },
    style: {
      imageBackgroundColour: "#FFFFFF",
      imageRoundedEdgeAmount: 50,
      borderStyle: "solid",
      borderWidth: 1,
      borderColour: "#888888",
      baseBackgroundType: "solid",
      baseBackgroundColour: "#DDDDDD",
      baseTextColour: "#000000",
      baseTextOutlineColour: "#111111",
      baseTextOutlineWidth: 1,
      baseRoundedEdgeAmount: 100,
      fontFamily: {
        title: "Georgia",
        value: 'Georgia, serif'
      },
      fontSize: 14,
    },
    sheet: {
      copies: 14,
      margins: 0,
      numbered: true,
      startNumber: 1,
    },
    imageBackgroundColourDialog: false,
    baseBackgroundColourDialog: false,
    borderColourDialog: false,
    baseTextColourDialog: false,
    baseTextOutlineColourDialog: false,
    presetStyles: [
      {
        name: "default",
        imageBackgroundColour: "#FFFFFF",
        imageRoundedEdgeAmount: 50,
        borderStyle: "solid",
        borderWidth: 1,
        borderColour: "#888888",
        baseBackgroundType: "solid",
        baseBackgroundColour: "#DDDDDD",
        baseTextColour: "#000000",
        baseTextOutlineColour: "#111111",
        baseTextOutlineWidth: 1,
        baseRoundedEdgeAmount: 100,
        fontFamily: {
          title: "Georgia",
          value: 'Georgia, serif'
        },
      },
      {
        name: "flat",
        imageBackgroundColour: "#FFFFFF",
        imageRoundedEdgeAmount: 0,
        borderStyle: "solid",
        borderWidth: 1,
        borderColour: "#888888",
        baseBackgroundType: "solid",
        baseBackgroundColour: "#FFFFFF",
        baseTextColour: "#000000",
        baseTextOutlineColour: "#111111",
        baseTextOutlineWidth: 1,
        baseRoundedEdgeAmount: 0,
        fontFamily: {
          title: "Lucida Console",
          value: '"Lucida Console", Courier, monospace'
        },
      }
    ],

    swatches: [
      ['#000000', '#888888', '#FFFFFF'],
      ['#FF0000', '#AA0000', '#550000'],
      ['#FFFF00', '#AAAA00', '#555500'],
      ['#00FF00', '#00AA00', '#005500'],
      ['#0000FF', '#0000AA', '#000055'],
    ],
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
    ],
    fontFamilies: [
      {
        title: "Times New Roman",
        value: '"Times New Roman", Times, serif'
      },
      {
        title: "Lucida Console",
        value: '"Lucida Console", Courier, monospace'
      },
      {
        title: "Bradley Hand",
        value: '"Bradley Hand", cursive'
      },
      {
        title: "Georgia",
        value: 'Georgia, serif'
      },
      {
        title: "Garamond",
        value: 'Garamond, serif'
      },
      {
        title: "Arial",
        value: 'Arial, Helvetica, sans-serif'
      },
      {
        title: "Tahoma",
        value: 'Tahoma, Verdana, sans-serif'
      },
      {
        title: "Brush Script",
        value: '"Brush Script MT", cursive'
      },
      {
        title: "Copperplate",
        value: 'Copperplate, Papyrus, fantasy'
      },
      {
        title: "Courier",
        value: '"Courier New", Courier, monospace'
      },
      {
        title: "Tangerine",
        value: "'Tangerine', serif"
      }
    ]
  }),
  computed: {
    fullTokenStyle() {
      return `margin: ${this.sheet.margins}in`
    },
    imageStyle() {
      const roundedEdgeMeasurement = this.sizes.filter(s => s.value === this.token.size)[0]!.width * (this.style.imageRoundedEdgeAmount / 200)
      return `border-style: ${this.style.borderStyle};` +
        `border-width: ${this.style.borderWidth}px;` +
        `border-top-left-radius: ${roundedEdgeMeasurement}in;` +
        `border-top-right-radius: ${roundedEdgeMeasurement}in;` +
        `background-color: ${this.style.imageBackgroundColour} !important; print-color-adjust: exact;` +
        `border-color: ${this.style.borderColour};`
    },
    tokenStyle() {

      return `border-style: ${this.style.borderStyle};` +
        `border-width: ${this.style.borderWidth}px;` +
        `border-color: ${this.style.borderColour};`

    },
    baseStyle() {
      const roundedEdgeMeasurement = this.sizes.filter(s => s.value === this.token.size)[0]!.width * (this.style.baseRoundedEdgeAmount / 200)
      switch (this.style.baseBackgroundType) {
        case "solid":
          return `background-color: ${this.style.baseBackgroundColour} !important; print-color-adjust: exact;` +
            `border-bottom-left-radius: ${roundedEdgeMeasurement}in;` +
            `border-bottom-right-radius: ${roundedEdgeMeasurement}in;` +
            `color: ${this.style.baseTextColour};` +
            `font-family: ${this.fontFamilies.filter(ff => ff.title === this.style.fontFamily.title)[0].value};` +
            `font-size: ${this.style.fontSize}px;` +
            `-webkit-text-stroke-width: ${this.style.baseTextOutlineWidth}px;` +
            `-webkit-text-stroke-color: ${this.style.baseTextOutlineColour};`
        default:
          return ""
      }
    }
  },
  methods: {
    resetStyle() {
      this.style.imageBackgroundColour = `#FFFFFF`
      this.style.imageRoundedEdgeAmount = 50
      this.style.baseBackgroundColour = `#DDDDDD`
      this.style.baseRoundedEdgeAmount = 100
      this.style.borderColour = `#888888`
      this.style.baseTextColour = `#000000`
      this.style.fontFamily = { title: "Georgia", value: 'Georgia, serif' }
    },
    randomiseStyle() {
      this.style.imageBackgroundColour = `#${Math.floor(Math.random() * 16777215).toString(16)}`
      this.style.imageRoundedEdgeAmount = Math.floor(Math.random() * 100)
      this.style.baseBackgroundColour = `#${Math.floor(Math.random() * 16777215).toString(16)}`
      this.style.baseRoundedEdgeAmount = Math.floor(Math.random() * 100)
      this.style.borderColour = `#${Math.floor(Math.random() * 16777215).toString(16)}`
      this.style.baseTextColour = `#${Math.floor(Math.random() * 16777215).toString(16)}`
      this.style.fontFamily = this.fontFamilies[Math.floor(Math.random() * this.fontFamilies.length)]
      // borderStyle: "solid",
      // borderWidth: 1,
      // baseBackgroundType: "solid",
      // baseBackgroundColour: "darkgrey",
      // baseRoundedEdgeAmount: 100,
    },
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
        windowTitle: `Paper Minis${this.token.name ? ' - ' + this.token.name : ''}`,
        autoClose: true
      })
      paperize()
    }
  },
})
</script>
