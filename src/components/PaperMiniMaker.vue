<template>
  <v-container class="fill-height">
    <v-responsive class="align-center text-center fill-height">
      <v-container fluid>
        <v-row>
          <v-col cols="6" md="4" class="no-print">
            <v-text-field v-model="imageUrl" label="Image URL"></v-text-field>
            <v-text-field v-model="name" label="Name"></v-text-field>
            
            <v-switch
              v-model="numbered"
              label="Numbered"
              hide-details
            ></v-switch>
            <p>Copies: {{ copies }}</p>
            <v-slider v-model="copies" :min="1" :max="20" :step="1" thumb-label></v-slider>
            <p>Size: {{ size }}</p>
            <v-radio-group v-model="size">
              <v-radio label="Tiny" value="tiny"></v-radio>
              <v-radio label="Medium/Small" value="medium"></v-radio>
              <v-radio label="Large" value="large"></v-radio>
              <v-radio label="Huge" value="huge"></v-radio>
              <v-radio label="Gargantuan" value="gargantuan"></v-radio>
            </v-radio-group>
            <p>Border style: {{ borderStyle }}</p>
            <v-radio-group v-model="borderStyle">
              <v-radio label="solid" value="solid"></v-radio>
              <v-radio label="dashed" value="dashed"></v-radio>
              <v-radio label="dotted" value="dotted"></v-radio>
            </v-radio-group>
            <p>Border width: {{ borderWidth }}</p>
            <v-slider v-model="borderWidth" :min="0" :max="5" :step="1" thumb-label></v-slider>
          </v-col>
          <v-col cols="12" md="4" class="d-flex justify-center align-center">
            <div v-for="(item, index) in copies" :key="index" style="float: left;">
              <div :style="`${tokenStyle} ${baseStyle}`" :class="`${size} ${size}-half-base`">
                <div v-if="name" class="rotated" >{{name}}</div><div v-else>&nbsp;</div>
                <div v-if="numbered" class="rotated" >{{index+1}}</div><div v-else>&nbsp;</div>
              </div>
              <v-img :style="tokenStyle" :class="`${size} flip-vertical`" :src="`${imageUrl}`" />
              <v-img :style="tokenStyle" :class="`${size}`" :src="`${imageUrl}`" />
              <div :style="`${tokenStyle} ${baseStyle}`" :class="`${size} ${size}-half-base`">
                <span v-if="numbered">{{index+1}}</span><div v-else>&nbsp;</div>
                <div v-if="name">{{name}}</div><div v-else>&nbsp;</div>
              </div>
              <div :style="tokenStyle" :class="`${size} ${size}-base`"></div>
            </div>
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
import VueHtmlToPaper from 'VueHtmlToPaper.vue'
export default defineComponent({
  name: 'PaperMiniMaker}',

  data: () => ({
    imageUrl: "https://www.dndbeyond.com/avatars/thumbnails/30783/955/1000/1000/638062024584880857.png",
    name: "Goblin",
    copies: 4,
    size: "medium",
    borderStyle: "solid",
    borderWidth: 1,
    baseBackgroundType: "solid",
    baseBackgroundColour: "lightgrey",
    numbered: true
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

})
</script>
