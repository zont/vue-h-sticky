<style scoped>
  .sticky-header > div:first-child {
    position: fixed;
    z-index: 2;
  }

  .sticky-header > div:nth-child(2),
  .sticky-header > div:nth-child(3) {
    position: fixed;
    z-index: 1;
  }
</style>

<template>
  <div class="sticky-header">
    <div ref="cross" :style="{top: top, left: left}">
      <slot name="cross"/>
    </div>
    <div ref="top" :style="{top: top, left: left, 'margin-left': marginLeft}">
      <slot name="top"/>
    </div>
    <div ref="left" :style="{top: top, left: left, 'margin-top': marginTop}">
      <slot name="left"/>
    </div>
    <div ref="body" :style="{'margin-left': crossWidth, 'margin-top': crossHeight}">
      <slot name="body"/>
    </div>
  </div>
</template>

<script>
export default {
  name: 'sticky-header',

  props: {
    fixes: {
      type: Array,
      default() {
        return [
          {
            property: 'width',
            from: 'left',
            to: 'cross',
            fromSelector: 'tbody tr:first-child td',
            toSelector: 'thead tr:last-child th'
          },
          {
            property: 'width',
            from: 'body',
            to: 'top',
            fromSelector: 'tbody tr:first-child td',
            toSelector: 'thead tr:last-child th'
          },
          {
            property: 'height',
            from: 'left',
            to: 'body',
            fromSelector: 'tbody tr td:last-child',
            toSelector: 'tbody tr td:first-child'
          }
        ];
      }
    }
  },

  data() {
    return {
      top: '0',
      left: '0',
      marginLeft: '0',
      marginTop: '0',
      crossWidth: '0',
      crossHeight: '0'
    };
  },

  mounted() {
    const rect = this.$el.getBoundingClientRect();

    this.top = `${rect.top}px`;
    this.left = `${rect.left}px`;

    this.fixes.forEach(fix => {
      fix.offsetProperty = `offset${fix.property[0].toUpperCase()}${fix.property.slice(1)}`;
    });

    this.listener = () => this.update();

    this.observer = new MutationObserver(() => {
      this.fixes.forEach(fix => this.fixCells(fix));
      this.listener();

      // Fix for style apply
      setTimeout(() => {
        this.fixes.forEach(fix => this.fixCells(fix));
        this.listener();
      }, 0);
    });
    this.observer.observe(this.$el, {
      childList: true,
      subtree: true,
      characterData: true
    });

    document.addEventListener('scroll', this.listener);
  },

  destroyed() {
    this.observer.disconnect();
    document.removeEventListener('scroll', this.listener);
  },

  methods: {
    update() {
      this.crossWidth = `${this.$refs.cross.clientWidth}px`;
      this.crossHeight = `${this.$refs.cross.clientHeight}px`;
      this.marginLeft = `${this.$refs.cross.clientWidth - window.pageXOffset}px`;
      this.marginTop = `${this.$refs.cross.clientHeight - window.pageYOffset}px`;
    },

    fixCells(fix) {
      const source = this.$refs[fix.from].querySelectorAll(fix.fromSelector);
      const destination = this.$refs[fix.to].querySelectorAll(fix.toSelector);

      if (source.length === destination.length) {
        source.forEach((cell, i) => {
          const sWidth = cell[fix.offsetProperty];
          const dWidth = destination[i][fix.offsetProperty];

          if (sWidth !== dWidth) {
            destination[i].style[fix.property] = `${sWidth}px`;
          }
        });
      }
    }
  }
};
</script>
