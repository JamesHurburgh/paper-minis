<template>
  <v-container class="fill-height">
    <v-responsive class="fill-height">
      <v-container fluid>
        <v-row>
          <v-col md="6">
            <v-card prepend-icon="mdi-cog">
              <template v-slot:title>
                Settings
              </template>
              <template v-slot:append>
                <v-btn prepend-icon="mdi-shuffle" @click="randomiseStyle"><div class="d-none d-lg-block">Random</div></v-btn>
                <v-btn prepend-icon="mdi-restart" @click="resetStyle"><div class="d-none d-lg-block">Reset</div></v-btn>
                <v-btn prepend-icon="mdi-printer" @click="print"><div class="d-none d-lg-block">Print</div></v-btn>
              </template>
              <v-card-subtitle>
                Adjust values here to change what the paper minis will look like.
              </v-card-subtitle>
              <v-card-text>
                <v-slider label="Copies" v-model="copies" :min="1" :max="20" :step="1" thumb-label></v-slider>
                <v-expansion-panels>
                  <v-expansion-panel key="Image">
                    <v-expansion-panel-title>
                      Token
                    </v-expansion-panel-title>
                    <v-expansion-panel-text>
                      <v-row>
                        <v-col md="9">
                          <v-text-field v-model="paperMini.imageUrl" label="Image URL"></v-text-field>
                        </v-col>
                        <v-col md="3">
                          <v-select label="Size" v-model="paperMini.size" :items="sizes"
                            :item-props="itemProps"></v-select>
                        </v-col>
                      </v-row>
                      <v-row>
                        <v-col md="9">
                          <v-slider label="Rounded Edges" v-model="miniStyle.imageRoundedEdgeAmount" :min="0"
                            :max="100" :step="1"
                            thumb-label></v-slider>
                        </v-col>
                        <v-col md="3">
                          <v-btn color="primary" append-icon="mdi-palette"><div class="d-none d-lg-block">Background</div>
                            <v-dialog v-model="imageBackgroundColourDialog" activator="parent" width="auto">
                              <v-card>
                                <v-card-title>
                                  <span class="text-h5">Select colour</span>
                                </v-card-title>
                                <v-card-text>
                                  <v-color-picker v-model="miniStyle.imageBackgroundColour" hide-inputs
                                    show-swatches :swatches="swatches"></v-color-picker>
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
                      <p>Style: {{ miniStyle.borderStyle }}</p>
                      <v-radio-group v-model="miniStyle.borderStyle">
                        <v-radio label="solid" value="solid"></v-radio>
                        <v-radio label="dashed" value="dashed"></v-radio>
                        <v-radio label="dotted" value="dotted"></v-radio>
                      </v-radio-group>
                      <v-slider label="Width" v-model="miniStyle.borderWidth" :min="0" :max="5" :step="1" thumb-label></v-slider>
                      
                      <v-col md="3">
                          <v-btn color="primary" append-icon="mdi-palette"><div class="d-none d-lg-block">Border</div>
                            <v-dialog v-model="borderColourDialog" activator="parent" width="auto">
                              <v-card>
                                <v-card-title>
                                  <span class="text-h5">Select colour</span>
                                </v-card-title>
                                <v-card-text>
                                  <v-color-picker v-model="miniStyle.borderColour"
                                    show-swatches :swatches="swatches"></v-color-picker>
                                </v-card-text>
                                <v-card-actions>
                                  <v-btn color="primary" block @click="borderColourDialog = false">Close
                                    Dialog</v-btn>
                                </v-card-actions>
                              </v-card>
                            </v-dialog>
                          </v-btn>
                        </v-col>
                    </v-expansion-panel-text>
                  </v-expansion-panel>
                  <v-expansion-panel key="Base">
                    <v-expansion-panel-title>
                      Base
                    </v-expansion-panel-title>
                    <v-expansion-panel-text>
                      <v-row>
                        <v-col md="6">
                          <v-text-field v-model="paperMini.name" label="Name" clearable></v-text-field>
                        </v-col>
                        <v-col md="3">
                          <v-switch v-model="miniStyle.numbered" label="Numbered"></v-switch>
                        </v-col>
                        <v-col md="3">
                          <v-text-field v-if="miniStyle.numbered" v-model="miniStyle.startNumber" persistent-hint hint="Start Number"
                            single-line type="number" />
                        </v-col>
                        <v-col md="9">
                          <v-slider label="Rounded Edges" v-model="miniStyle.baseRoundedEdgeAmount" :min="0"
                            :max="100" :step="1" thumb-label></v-slider>
                        </v-col>
                        <v-col md="3">
                          <v-btn color="primary" append-icon="mdi-palette"><div class="d-none d-lg-block">Background</div>
                            <v-dialog v-model="baseBackgroundColourDialog" activator="parent" width="auto">
                              <v-card>
                                <v-card-title>
                                  <span class="text-h5">Select base colour</span>
                                </v-card-title>
                                <v-card-text>
                                  <v-color-picker v-model="miniStyle.baseBackgroundColour"
                                    show-swatches :swatches="swatches"></v-color-picker>
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
                    <div :style="`${tokenStyle} ${reversedBaseStyle}`" :class="`${paperMini.size} ${paperMini.size}-half-base`">
                      <div v-if="miniStyle.numbered" class="rotated">{{ index + Number(miniStyle.startNumber) }}</div>
                      <div v-else>&nbsp;</div>
                      <div v-if="paperMini.name" class="rotated">{{ paperMini.name }}</div>
                      <div v-else>&nbsp;</div>
                    </div>
                    <v-img :style="imageStyle" :class="`${paperMini.size} flip-vertical`"
                      :src="`${paperMini.imageUrl}`" />
                    <v-img :style="imageStyle" :class="`${paperMini.size}`" :src="`${paperMini.imageUrl}`" />
                    <div :style="`${tokenStyle} ${baseStyle}`" :class="`${paperMini.size} ${paperMini.size}-half-base`">
                      <div v-if="paperMini.name">{{ paperMini.name }}</div>
                      <div v-else>&nbsp;</div>
                      <div v-if="miniStyle.numbered">{{ index + Number(miniStyle.startNumber) }}</div>
                      <div v-else>&nbsp;</div>
                    </div>
                    <div :style="tokenStyle" :class="`${paperMini.size} ${paperMini.size}-base`"></div>
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
    paperMini: {
      imageUrl: "https://i.imgur.com/bYJ3gBl.png",
      name: "Werewolf",
      size: "medium",
    },
    miniStyle: {
      imageBackgroundColour: "#FFFFFF",
      imageRoundedEdgeAmount: 50,
      borderStyle: "solid",
      borderWidth: 1,
      borderColour: "#888888",
      baseBackgroundType: "solid",
      baseBackgroundColour: "#DDDDDD",
      baseRoundedEdgeAmount: 100,
      numbered: true,
      startNumber: 1,
    },
    copies: 14,
    imageBackgroundColourDialog: false,
    baseBackgroundColourDialog: false,
    borderColourDialog: false,
    
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
    ]
  }),
  computed: {
    imageStyle() {
      const roundedEdgeMeasurement = this.sizes.filter(s => s.value === this.paperMini.size)[0]!.width * (this.miniStyle.imageRoundedEdgeAmount / 200)
      return `border-style: ${this.miniStyle.borderStyle};` +
        `border-width: ${this.miniStyle.borderWidth}px;` +
        `border-top-left-radius: ${roundedEdgeMeasurement}in;` +
        `border-top-right-radius: ${roundedEdgeMeasurement}in;` +
        `background-color: ${this.miniStyle.imageBackgroundColour} !important; print-color-adjust: exact;` +
        `border-color: ${this.miniStyle.borderColour};`
    },
    tokenStyle() {

      return `border-style: ${this.miniStyle.borderStyle};` +
        `border-width: ${this.miniStyle.borderWidth}px;` +
        `border-color: ${this.miniStyle.borderColour};`
        
    },
    baseStyle() {
      const roundedEdgeMeasurement = this.sizes.filter(s => s.value === this.paperMini.size)[0]!.width * (this.miniStyle.baseRoundedEdgeAmount / 200)
      switch (this.miniStyle.baseBackgroundType) {
        case "solid":
          return `background-color: ${this.miniStyle.baseBackgroundColour} !important; print-color-adjust: exact;` +
        `border-bottom-left-radius: ${roundedEdgeMeasurement}in;` +
        `border-bottom-right-radius: ${roundedEdgeMeasurement}in;`
        default:
          return ""
      }
    },
    reversedBaseStyle() {
      const roundedEdgeMeasurement = this.sizes.filter(s => s.value === this.paperMini.size)[0]!.width * (this.miniStyle.baseRoundedEdgeAmount / 200)
      switch (this.miniStyle.baseBackgroundType) {
        case "solid":
          return `background-color: ${this.miniStyle.baseBackgroundColour} !important; print-color-adjust: exact;` +
        `border-top-left-radius: ${roundedEdgeMeasurement}in;` +
        `border-top-right-radius: ${roundedEdgeMeasurement}in;`
        default:
          return ""
      }

    }
  },
  methods: {
    resetStyle(){
      this.miniStyle.imageBackgroundColour = `#FFFFFF`
      this.miniStyle.imageRoundedEdgeAmount = 50
      this.miniStyle.baseBackgroundColour = `#DDDDDD`
      this.miniStyle.baseRoundedEdgeAmount = 100
      this.miniStyle.borderColour = `#888888`
    },
    randomiseStyle() {
      this.miniStyle.imageBackgroundColour = `#${Math.floor(Math.random()*16777215).toString(16)}`
      this.miniStyle.imageRoundedEdgeAmount = Math.floor(Math.random()*100)
      this.miniStyle.baseBackgroundColour = `#${Math.floor(Math.random()*16777215).toString(16)}`
      this.miniStyle.baseRoundedEdgeAmount = Math.floor(Math.random()*100)
      this.miniStyle.borderColour = `#${Math.floor(Math.random()*16777215).toString(16)}`
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
        windowTitle: `Paper Minis${this.paperMini.name ? ' - ' + this.paperMini.name : ''}`,
        autoClose: true
      })
      paperize()
    }
  },
})
</script>
