<script>
export default {
  props: {
    data: {
      type: Object,
      default: () => {
        return {
          current_page: 1,
          data: [],
          from: 1,
          last_page: 1,
          next_page_url: null,
          per_page: 10,
          prev_page_url: null,
          to: 1,
          total: 0
        };
      }
    },
    limit: {
      type: Number,
      default: 0
    },
    pageNumberLimit: {
      type: Number,
      default: 0
    }
  },

  computed: {
    pageRange() {
      if (this.limit === -1) {
        return 0;
      }

      if (this.limit === 0) {
        return this.data.last_page;
      }

      var current = this.data.current_page;
      var last = this.getlastPage();
      var delta = this.limit;
      var left = current - delta;
      var right = current + delta + 1;
      var range = [];
      var pages = [];
      var l;

      for (var i = 1; i <= last; i++) {
        if (i === 1 || i === last || (i >= left && i < right)) {
          range.push(i);
        }
      }

      range.forEach(function(i) {
        if (l) {
          if (i - l === 2) {
            pages.push(l + 1);
          } else if (i - l !== 1) {
            pages.push("...");
          }
        }
        pages.push(i);
        l = i;
      });

      return pages;
    },
  },

  methods: {
    previousPage() {
      this.selectPage(--this.data.current_page);
    },
    nextPage() {
      this.selectPage(++this.data.current_page);
    },
    firstPage() {
      this.selectPage(1);
    },
    lastPage() {
      this.selectPage(this.getlastPage());
    },
    selectPage(page) {
      if (page === "...") {
        return;
      }

      this.$emit("pagination-change-page", page);
    },
    getlastPage() {
        var last =  this.data.last_page;
      if ( this.pageNumberLimit > 0 && this.data.last_page > this.pageNumberLimit) {
        last = this.pageNumberLimit;
      }

      return last;
    }
  },

  render() {
    return this.$scopedSlots.default({
      data: this.data,
      limit: this.limit,
      pageRange: this.pageRange,
      prevButtonEvents: {
        click: e => {
          e.preventDefault();
          this.previousPage();
        }
      },
      nextButtonEvents: {
        click: e => {
          e.preventDefault();
          this.nextPage();
        }
      },
      firstButtonEvents: {
        click: e => {
          e.preventDefault();
          this.firstPage();
        }
      },
      lastButtonEvents: {
        click: e => {
          e.preventDefault();
          this.lastPage();
        }
      },
      pageButtonEvents: page => ({
        click: e => {
          e.preventDefault();
          this.selectPage(page);
        }
      }),
      getlastPage :  this.getlastPage
    });
  }
};
</script>
