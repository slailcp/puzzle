<template>
  <div class="p-item p-bg" :style="itemstyle">
    <svg :style="svgtyle" width="725" height="725" viewBox="0 0 725 725" fill="none" xmlns="http://www.w3.org/2000/svg">
      <g :filter="`url(#${shadowColor ? 'filter0_i_5_2_' + mark + index : ''})`">
        <path fill-rule="evenodd" clip-rule="evenodd" :d="path" :fill="colors.length ? `url(#paint0_linear_3_18_${mark + index})` : bgColor" :stroke="borderColor" :stroke-width="borderWidth" />
      </g>

      <mask :id="`myMask_${mark + index}`" v-if="imgpath">
        <rect width="100%" height="100%" fill="black" />
        <path fill-rule="evenodd" clip-rule="evenodd" :d="path" fill="white" :stroke="borderColor" :stroke-width="borderWidth" />
      </mask>
      <image v-if="imgpath" x="0" y="0" width="725" height="725" :href="imgpath" :mask="`url(#myMask_${mark + index})`" />

      <defs>
        <linearGradient v-if="colors.length && gradientobj.type == 'linear'" :id="`paint0_linear_3_18_${mark + index}`" v-bind="gradientobj.point" gradientUnits="userSpaceOnUse">
          <stop :offset="fi / colors.length" :stop-color="f" :stop-opacity="gradientobj.opacity" v-for="(f, fi) of colors" :key="fi" :style="gradientobj.style" />
        </linearGradient>

        <radialGradient v-if="colors.length && gradientobj.type == 'radial'" :id="`paint0_linear_3_18_${mark + index}`" v-bind="gradientobj.point">
          <stop :offset="fi / colors.length" :stop-color="f" :stop-opacity="gradientobj.opacity" v-for="(f, fi) of colors" :key="fi" :style="gradientobj.style" />
        </radialGradient>

        <filter v-if="shadowColor" :id="`filter0_i_5_2_${mark + index}`" x="0" y="0" width="745" height="745" filterUnits="userSpaceOnUse" color-interpolation-filters="sRGB">
          <feFlood flood-opacity="0" result="BackgroundImageFix" />
          <feBlend mode="normal" in="SourceGraphic" in2="BackgroundImageFix" result="shape" />
          <feColorMatrix in="SourceAlpha" type="matrix" values="0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 127 0" result="hardAlpha" />
          <feOffset dx="20" dy="20" />
          <feGaussianBlur :stdDeviation="shadowSpread" />
          <feComposite in2="hardAlpha" operator="arithmetic" k2="-1" k3="1" />
          <feColorMatrix type="matrix" :values="generateColorMatrixValues(shadowColor)" />
          <feBlend mode="normal" in2="shape" result="effect1_innerShadow_5_2" />
        </filter>
      </defs>
    </svg>
    <div class="content"><slot></slot></div>
  </div>
</template>
<script setup>
import { ref } from "vue";
const props = defineProps({
  // type: {
  //   type: String,
  //   default: "", // fill,shadow
  // },
  bgColor: {
    type: String,
    default: "#F2F2F3",
  },
  borderColor: {
    type: String,
    default: "#3E5185",
  },
  borderWidth: {
    type: [Number, String],
    default: 2,
  },
  index: {
    type: [Number, String],
    default: 0,
  },

  shadowColor: {
    type: String,
    default: "",
  },
  // 向内阴影
  shadowRatio: {
    type: [Number, String],
    default: 0.2,
  },
  // 扩散
  shadowSpread: {
    type: [Number, String],
    default: 125,
  },
  colors: {
    type: Array,
    default: () => {
      return [];
    },
  },

  total: {
    // 一共多少个
    type: [Number, String],
    default: 0,
  },
  linenum: {
    // 一行多少个
    type: [Number, String],
    default: 6,
  },
  size: {
    type: String,
    default: "100px",
  },
  style: {
    type: Object,
    default: () => {
      return {};
    },
  },
  // 当前拼图的标识，防止重复
  mark: {
    type: String,
    default: "one",
  },
  imgpath: {
    type: String,
    default: "",
  },
  gradient: {
    type: Object,
    default: (e, q) => {
      return {};
    },
  },
});

const gradientobj = ref({
  type: props.gradient.type || "linear", // type = radial,point = cx="50%" cy="50%" r="50%" fx="50%" fy="50%"
  point: props.gradient.point || {
    x1: "362.5",
    y1: "725",
    x2: "362.5",
    y2: "0",
  }, // type = line,point= x1="362.5" y1="725" x2="362.5" y2="0"
  opacity: props.gradient.opacity || 1,
});

let path = ref("M375 75C375 94.2089 367.779 111.731 355.903 125H600V373.097C613.269 361.221 630.791 354 650 354C691.421 354 725 387.579 725 429C725 470.421 691.421 504 650 504C630.791 504 613.269 496.779 600 484.903V725H359.903C371.779 711.731 379 694.209 379 675C379 633.579 345.421 600 304 600C262.579 600 229 633.579 229 675C229 694.209 236.221 711.731 248.097 725H0V484.903C13.2689 496.779 30.7911 504 50 504C91.4214 504 125 470.421 125 429C125 387.579 91.4214 354 50 354C30.7911 354 13.2689 361.221 0 373.097V125H244.097C232.221 111.731 225 94.2089 225 75C225 33.5786 258.579 0 300 0C341.421 0 375 33.5786 375 75Z");

if (props.index >= props.total - props.linenum && props.total) {
  path.value = "M375 75C375 94.2089 367.779 111.731 355.903 125H600V369.097C613.269 357.221 630.791 350 650 350C691.421 350 725 383.579 725 425C725 466.421 691.421 500 650 500C630.791 500 613.269 492.779 600 480.903V725H0V480.903C13.2689 492.779 30.7911 500 50 500C91.4214 500 125 466.421 125 425C125 383.579 91.4214 350 50 350C30.7911 350 13.2689 357.221 0 369.097V125H244.097C232.221 111.731 225 94.2089 225 75C225 33.5786 258.579 0 300 0C341.421 0 375 33.5786 375 75Z";
}
if (props.index % props.linenum == 0 && props.total) {
  path.value = "M355.903 125C367.779 111.731 375 94.2089 375 75C375 33.5786 341.421 0 300 0C258.579 0 225 33.5786 225 75C225 94.2089 232.221 111.731 244.097 125H0V725H244.097C232.221 711.731 225 694.209 225 675C225 633.579 258.579 600 300 600C341.421 600 375 633.579 375 675C375 694.209 367.779 711.731 355.903 725H600V480.903C613.269 492.779 630.791 500 650 500C691.421 500 725 466.421 725 425C725 383.579 691.421 350 650 350C630.791 350 613.269 357.221 600 369.097V125H355.903Z";
}

if (props.index >= props.total - props.linenum && props.index % props.linenum == 0 && props.total) {
  path.value = "M375 75C375 94.2089 367.779 111.731 355.903 125H600V369.097C613.269 357.221 630.791 350 650 350C691.421 350 725 383.579 725 425C725 466.421 691.421 500 650 500C630.791 500 613.269 492.779 600 480.903V725H0V125H244.097C232.221 111.731 225 94.2089 225 75C225 33.5786 258.579 0 300 0C341.421 0 375 33.5786 375 75Z";
}

// const gradientobj = ref({
//   x1: "362.5",
//   y1: "725",
//   x2: "362.5",
//   y2: "0",
// });

const itemstyle = ref({
  ...props.style,
  width: props.size,
  height: props.size,
});
0.666 * 0.208333 + 0.666;
// const re = new RegExp(props.unit, "g");
// const w = Number(itemstyle.value.width.replace(re, ""));
const unit = props.size.replace(/\d+|\./g, "");
const w = Number(props.size.replace(/[^\-?\d\.]/g, ""));

const svgtyle = ref({
  width: (w * (125 / 600) + w).toFixed(4) + unit,
  height: (w * (125 / 600) + w).toFixed(4) + unit,
  top: -(w * (125 / 600).toFixed(4)) + unit,
});

function generateColorMatrixValues(colorHex) {
  // 将十六进制颜色转换为RGB数组
  const r = parseInt(colorHex.slice(1, 3), 16) / 255;
  const g = parseInt(colorHex.slice(3, 5), 16) / 255;
  const b = parseInt(colorHex.slice(5, 7), 16) / 255;

  // 返回对应的feColorMatrix values
  return `0 0 0 0 ${r} 0 0 0 0 ${g} 0 0 0 0 ${b} 0 0 0 ${props.shadowRatio} 0`;
}
</script>

<style lang="less" scoped>
.p-item {
  position: relative;
  display: inline-block;font-size: 14px;
}
.p-item svg {
  position: absolute;
  // top: 50%;
  // left: 50%;
  // transform: translate(-50%, -50%);
}

.item svg {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}

.content {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
  height: 100%;
  color: #000;
  position: absolute;
  z-index: 1;
}
</style>
