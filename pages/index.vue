<template>
  <div>
    <div id="capture" ref="element" style="padding: 10px; background: #f5da55">
      <h4 style="color: #000">ប្រវត្តិសាស្ត្រ</h4>
      <form >
        <label for="fname">First name:</label>
        <input type="text" id="fname" name="fname"><br><br>
        <label for="lname">Last name:</label>
        <input type="text" id="lname" name="lname"><br><br>
        <input type="submit" value="Submit">
      </form>
    </div>

    <br />
    <br />
    <button @click="share">share please</button>
  </div>
</template>




<script>
import html2canvas from 'html2canvas'

export default {
  methods: {
    // iife here!
    share() {
      ;(async () => {
        if (!('share' in navigator)) {
          return
        }
        // `element` is the HTML element you want to share.
        // `backgroundColor` is the desired background color.
        const canvas = await html2canvas(this.$refs.element)
        canvas.toBlob(async (blob) => {
          // Even if you want to share just one file you need to
          // send them as an array of files.
          const files = [new File([blob], 'image.png', { type: blob.type })]
          const shareData = {
            text: 'Some text',
            title: 'Some title',
            files,
          }
          if (navigator.canShare(shareData)) {
            try {
              await navigator.share(shareData)
            } catch (err) {
              if (err.name !== 'AbortError') {
                console.error(err.name, err.message)
              }
            }
          } else {
            console.warn('Sharing not supported', shareData)
          }
        })
      })()
    },
  },
}
</script>
