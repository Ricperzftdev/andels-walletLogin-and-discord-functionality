<script setup lang="ts">
import { onMounted, onBeforeUnmount } from "vue";
import { Frame, Button, Rectangle, BitmapColor, Pic, TextArea } from "zimjs";

let frame = null;

class App {
  data = null;
  bg = null;
  icon = null;
  scanWalletEetrnlBtn = null;
  scanWalletNamiBtn = null;
  mintBtn = null;
  walletContentTxt = null;

  constructor() {
    // super();
    let p = this;

    // init Frame
    var scaling = "fit";
    var width = 512;
    var height = 1024;
    frame = new Frame(scaling, width, height, "#333", "#111");

    frame.on("ready", () => {
      zog("fr = " + frame.stage);
      p.loadAssets();
    });
  }

  async loadJson(fileName) {
    const response = await fetch(fileName);
    return await response.json();
  }

  async loadAssets() {
    zog("loadAssets");
    let p = this;

    p.data = await this.loadJson("./assets/config/config.json");

    // load graphics
    let assetsToLoad = [
      "assets/graphics/_ui/texture_wood_bw.jpg",
      "assets/graphics/_ui/paper_bw.jpg",
      "assets/graphics/_ui/icon.png",
    ];
    let loader = frame.loadAssets(assetsToLoad);
    loader.on(
      "complete",
      function () {
        zog("finished loading");
        p.start();
      },
      null,
      true
    );
  }

  start() {
    zog("start");
    let p = this;

    p.bg = new Rectangle(
      512,
      1024,
      new BitmapColor("assets/graphics/_ui/texture_wood_bw.jpg")
    );
    frame.stage.addChild(this.bg);

    // add a rectangle
    /*let rect = new Rectangle({width: 96, height: 96, color: red, corner: 8}).pos(0, 32, CENTER, TOP);
    frame.stage.addChild(rect);*/

    zog(p.data['title']);

    p.icon = new Pic("assets/graphics/_ui/icon.png").pos(0, 32, CENTER, TOP);
    frame.stage.addChild(this.icon);

    // add scanWalletEetrnlBtn
    p.scanWalletEetrnlBtn = new Button(320, 60, "Scan Wallet Eternl").pos(
      0,
      144,
      CENTER,
      TOP
    );
    frame.stage.addChild(p.scanWalletEetrnlBtn);
    p.scanWalletEetrnlBtn.on("click", function () {
      p.getWalletTokens("Eternl");
    });

    // add scanWalletNamiBtn
    p.scanWalletNamiBtn = new Button(320, 60, "Scan Wallet Nami").pos(
      0,
      224,
      CENTER,
      TOP
    );
    frame.stage.addChild(p.scanWalletNamiBtn);
    p.scanWalletNamiBtn.on("click", function () {
      p.getWalletTokens("Nami");
    });

    // add mintBtn
    p.mintBtn = new Button(320, 60, "Call Mint").pos(0, 304, CENTER, TOP);
    frame.stage.addChild(p.mintBtn);
    p.mintBtn.on("click", function () {
      p.mintTokens();
    });

    // walletContentTxt
    p.walletContentTxt = new TextArea(448, 512).pos(0, 32, CENTER, BOTTOM);
    frame.stage.addChild(p.walletContentTxt);
    p.walletContentTxt.text = 'from config file: ' + p.data['title'];

    frame.stage.update();
  }

  getWalletTokens(walletType) {
    let p = this;

    // print to console
    zog("getWalletTokens " + walletType);

    // alert
    alert("getWalletTokens " + walletType);

    // call getWalletTokens
    let tokens = "$nuxt.$store.getters.getWalletTokens";
    p.walletContentTxt.text = tokens;
  }

  mintTokens() {
    let p = this;

    // print to console
    zog("mintTokens");

    // alert
    alert("mintTokens");
  }
}

onMounted(() => {
  new App();
});

onBeforeUnmount(() => {
  frame.dispose();
});
</script>

<template>
</template>
