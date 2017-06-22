<template>
  <svg 
    :height="height" 
    :width="width" 
    @click="addPoint" 
    @contextmenu="clearPoints" 
    :style="svgStyle">
  
    <polygon 
      :points="pointsString" 
      :fill="fill" 
      :fill-opacity="fillOpacity" 
      :stroke="stroke" 
      :stroke-width="strokeWidth" />
  
    <circle 
      v-for="point in points" 
      :cx="point.x" 
      :cy="point.y" 
      :r="pointsRadius" 
      :fill="getPointFill(point)" 
      :fill-opacity="getPointFillOpacity(point)" 
      @click="removePointHoveringOn" 
      @mouseenter="hoveringOn(point)" 
      @mouseleave="hoveringOn(null)" />

  </svg>
</template>

<script>
export default {
  
  name: 'polygon-editor',
  
  props: {
    value: { type: String, default: null },
    height: { type: String, default: "100%" },
    width: { type: String, default: "100%" },
    pointsRadius: { type: Number, default: 5 },
    pointsHoverColor: { type: String, default: "red" },
    pointsColor: { type: String, default: "teal" },
    fill: { type: String, default: "lightgray" },
    fillOpacity: { type: Number, default: 0.5 },
    stroke: { type: String, default: "gray" },
    strokeWidth: { type: Number, default: 1 },
    backgroundImage: { type: String, default: null }
  },
  
  data() {
    return {
      points: this.extractPoints(this.value),
      pointHoveringOn: null
    }
  },

  computed: {
    hasPoints() {
      return this.points && this.points.length > 0;
    },
    lastPoint() {
      if (this.hasPoints) {
        return this.points[this.points.length - 1];
      }
      return null;
    },
    pointsString() {
      let pointStrings = this.points.map(function (point) {
        return point.x + ',' + point.y;
      }, this);
      return pointStrings.join(' ');
    },
    svgStyle() {
      let style = '';
      if (this.backgroundImage) {
        style += 'background:url("' + this.backgroundImage + '") no-repeat left top'
      }
      return style;
    }
  },

  watch: {
    pointsString() {
      this.$emit('input', this.pointsString)
    }
  },

  methods: {
    extractPoints() {
      const valuePattern = /^(\d+,\d+ )*\d+,\d+$/;
      let points = [];
      if (!this.value) { return points; }
      if (!valuePattern.test(this.value)) {
        console.warn('Value provided is not in the correct format. Value must bew in the format of polygon "points" attribute (https://developer.mozilla.org/en-US/docs/Web/SVG/Attribute/points)');
        return points;
      }
      this.value.split(' ').forEach(point => {
        points.push(
          {
            x: parseInt(point.split(',')[0], 10),
            y: parseInt(point.split(',')[1], 10),
          }
        );
      }, this);
      return points;
    },
    addPoint(evt) {
      this.points.push(
        {
          x: evt.offsetX,
          y: evt.offsetY
        }
      )
    },
    removePointHoveringOn(evt) {
      if (!this.pointHoveringOn) {
        return;
      }
      evt.stopImmediatePropagation();
      this.points.splice(this.points.indexOf(this.pointHoveringOn), 1);
    },
    clearPoints(evt) {
      this.points = [];
      evt.preventDefault();
    },
    getPointFill(point) {
      if (point === this.pointHoveringOn) {
        return this.pointsHoverColor;
      }
      return this.pointsColor;
    },
    getPointFillOpacity(point) {
      if (point === this.pointHoveringOn || point === this.lastPoint) {
        return 1;
      }
      return 0;
    },
    hoveringOn(point) {
      this.pointHoveringOn = point;
    },
  }
  
}

</script>

<style lang="scss">

</style>
