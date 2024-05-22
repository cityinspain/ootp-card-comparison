<template>
  <div class="progress-container">
    <div class="progress-bar" :style="progressStyle">
      <!-- Display the value inside the progress bar -->
      <span class="value-label">{{ value }}</span>
    </div>
  </div>
</template>

<script>
export default {
  name: 'RatingBar',
  props: {
    value: {
      type: Number,
      required: true
    },
    max: {
      type: Number,
      required: false
    }
  },
  computed: {
    progressStyle() {
      const getColor = (v, max) => {
        const hues = [0, 60, 120, 180, 240] // Corresponds to red, yellow, green, cyan, deep blue

        // Calculate the index position in the hues array based on the value
        const index = (v / max) * (hues.length - 1)

        // Find the lower and upper bounds for interpolation
        const lowerIndex = Math.floor(index)
        const upperIndex = Math.ceil(index)
        const ratio = index - lowerIndex

        // Interpolate the hue value
        const hue = hues[lowerIndex] + ratio * (hues[upperIndex] - hues[lowerIndex])

        // Return the HSL color string
        return `hsl(${hue}, 100%, 50%)`
      }

      const max = this.max || 120
      const percentage = (this.value / max) * 100
      //   const hue = (percentage * 240).toFixed(0) // Scale hue from 0 (red) to 240 (blue)
      return {
        width: `${percentage}%`,
        // backgroundColor: `hsl(${hue}, 100%, 50%)`
        background: getColor(this.value, max)
      }
    }
  }
}
</script>

<style scoped>
.progress-container {
  width: 100%;
  height: 20px;
  background-color: #eee;
  /* border-radius: 10px; */
  overflow: hidden;
  position: relative; /* Needed for positioning the value label */
}

.progress-bar {
  height: 100%;
  /* border-radius: 10px; */
  position: relative; /* Needed for absolute positioning of the label */
  display: flex; /* Ensures content is aligned properly */
  align-items: center; /* Vertically centers the label */
}

.value-label {
  position: absolute;
  left: 0;
  background-color: rgba(0, 0, 0, 0.5); /* Semi-transparent black backdrop */
  color: white;
  padding: 0 8px; /* Padding around the text */
  height: 100%;
  display: flex;
  align-items: center; /* Vertically center the text within the label */
  /* border-top-left-radius: 10px; */
  /* border-bottom-left-radius: 10px; */
}
</style>
