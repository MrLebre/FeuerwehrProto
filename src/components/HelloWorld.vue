<template>
  <v-container>
    <v-img
      :src="require('@/assets/zimmer.png')"
      id="imgZimmer"
      @click="onImageClick"
    />

    <v-dialog v-model="opened" width="50%">
      <v-card>
        <v-card-title>{{msg.title}}</v-card-title>
        <v-card-text>
          {{msg.text}}
        </v-card-text>
        <v-card-actions>
          <v-spacer/>
          <v-btn v-for="(o, i) in msg.options" :key="i">
            {{o}}
          </v-btn>
          <v-spacer/>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </v-container>
</template>

<script lang="ts">
import { Vue, Component, Watch } from "vue-property-decorator";

@Component
export default class HelloWorld extends Vue {
  private opened = false;
  private msg: any = {};

  private msgs = [
    { xLower: 0.0, xUpper: 0.5, yLower: 0.0, yUpper: 0.5, title: "Fenster",  text: "Willst du das Fenster öffnen?", options: ["Öffnen", "Schließen"]},
  ];

  private onImageClick(event: MouseEvent) {
    const elem = document.getElementById("imgZimmer");
    
    if (elem != null) {
      let bb = elem.getBoundingClientRect();

      const x = event.offsetX;
      const y = event.offsetY;

      const xp = x / bb.width;
      const yp = y / bb.height;
      console.log(`x: ${x} (${xp}), y: ${y} (${yp})`);

      for (const msg of this.msgs) {
        if (
          xp > msg.xLower &&
          xp < msg.xUpper &&
          yp > msg.yLower &&
          yp < msg.yUpper
        ) {
          this.opened = true;
          this.msg = msg;
          break;
        }
      }
    }
  }
}
</script>
