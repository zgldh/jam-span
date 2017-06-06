<template>
  <span class="jam-span" v-html="displayHtml"></span>
</template>

<script type="javascript">
  export default {
    props: {
      text: {
        type: String,
        default: ''
      },
      duration: {
        type: Number,
        default: 1000
      },
      interval: {
        type: Number,
        default: 10
      }
    },
    data: function () {
      return {
        displayHtml: '',
        intervalHandle: null,
        times: [],
        targets: []
      };
    },
    mounted: function () {
      this.startTimer();
    },
    watch: {
      text: function (newText) {
        this.startTimer();
      }
    },
    methods: {
      startTimer: function () {
        if (this.intervalHandle) {
          clearInterval(this.intervalHandle);
        }

        this.times = [];
        this.targets = [];
        var startText = '';
        var textLen = this.text.length;
        for (var i = 0; i < textLen; i++) {
          this.targets[i] = this.text.charCodeAt(i);
          this.times[i] = Math.floor(Math.random() * this.duration / this.interval);
          var char = String.fromCharCode(this.targets[i] + this.times[i]);
          startText += char;
        }

        this.intervalHandle = setInterval(() => {
          var currentText = '';
          var allZero = true;
          for (var i = 0, len = this.times.length; i < len; i++) {
            var char = null;
            if (this.times[i] == 0) {
              char = String.fromCharCode(this.targets[i]);
            } else {
              allZero = false;
              if (this.targets[i] <= 126) {
                char = String.fromCharCode(Math.floor(Math.random() * 95 + 32));
              } else {
                char = String.fromCharCode(this.targets[i] + Math.floor(Math.random() * 100));
              }
              char = '<span class="jam-code">' + char + '</span>';
              this.times[i]--;
            }
            currentText += char;
          }
          this.displayHtml = currentText;
          if (allZero) {
            clearInterval(this.intervalHandle);
          }
        }, this.interval);

        this.displayHtml = startText;
      }
    }
  };
</script>

<style lang="scss">
  .jam-span .jam-code {
    color: red;
  }
</style>
