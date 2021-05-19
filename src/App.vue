<template>
    <span>
        <trading-vue
            title-txt="BTCUSD"
            ref="tvjs"
            :data="dc"
            :index-based="index_based"
            :width="this.width"
            :height="this.height"
            :toolbar="true"
            :extensions="ext"
            :overlays="overlays"
            :legend-buttons="[
                'display',
                'settings',
                'up',
                'down',
                'add',
                'remove',
            ]"
            :chart-config="{
                TB_B_STYLE: 'line',
                TB_BORDER: 1,
                TB_ICON_BRI: 1.5,
                DEFAULT_LEN: 100,
            }"
            :color-back="colors.back"
            :color-grid="colors.grid"
            :color-text="colors.text"
            :color-cross="colors.cross"
            :color-candle-dw="colors.candle_dw"
            :color-wick-dw="colors.wick_dw"
            :color-title="colors.tvTitle"
        >
        </trading-vue>
        <span class="night-mode">
            <input type="checkbox" v-model="night" />
            <label>Night Mode</label>
        </span>
        <span class="gc-mode">
            <input type="checkbox" v-model="index_based" />
            <label>Index Based</label>
        </span>
    </span>
</template>

<script>
import { TradingVue, DataCube } from "trading-vue-js";
import XP from "tvjs-xp";
import Overlays from "tvjs-overlays";
import Data from "../data/data.xp.json";

export default {
    name: "app",
    components: { TradingVue },
    methods: {
        onResize(event) {
            this.width = window.innerWidth;
            this.height = window.innerHeight;
        },
        add() {
            this.chart.merge("chart.data", Data.ohlcv.slice());
        },
    },
    mounted() {
        window.addEventListener("resize", this.onResize);
        this.onResize();
        window.tv = this.$refs.tvjs;
        window.test = this;
    },
    beforeDestroy() {
        window.removeEventListener("resize", this.onResize);
    },
    data() {
        return {
            overlays: Object.values(Overlays),
            ext: Object.values(XP),
            dc: new DataCube(Data),
            width: window.innerWidth,
            height: window.innerHeight,
            night: true,
            index_based: false,
        };
    },
    computed: {
        colors() {
            return this.night
                ? {
                      back: "#121827",
                      grid: "#3e3e3e",
                      text: "#35a776",
                      cross: "#dd64ef",
                      candle_dw: "#e54077",
                      wick_dw: "#e54077",
                  }
                : {
                      back: "#fff",
                      grid: "#eee",
                      text: "#333",
                      candle_dw: "black",
                      wick_dw: "black",
                  };
        },
    },
};
</script>
