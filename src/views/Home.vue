<template>
  <div id="home">
    <Header HLight="top" />
    <div :class="'position-fixed p-1 m-1 scroll bg-white rounded ' + marginTop">
      <h1>{{ Math.floor(scroll).toLocaleString()+'px' }}</h1>
    </div>
    <div :class="'position-fixed p-1 m-1 timer bg-white rounded ' + marginTop">
      <h1>{{ this.timerInt }}<small class="font-weight-normal">{{ this.timerFloot }}</small></h1>
    </div>
    <div class="container p-3">
    <Article
      v-for="article_param in article_params"
      :key="article_param.id"
      :headding="article_param.headding"
      :yt_title="article_param.yt_title"
      :yt_desc="article_param.yt_desc"
      :article_desc="article_param.article_desc"
      :article_cont="article_param.article_cont"
    />
    </div>
    <Modal @scroll_start="scroll_start" />
    <ResultModal v-if="isFinished" :result="result" />
  </div>
</template>

<script>
import Article from '../components/Article.vue'
import Modal from '../components/Modal.vue'
import ResultModal from '../components/ResultModal.vue'
import Header from '../components/Header.vue'

let headdings = [
  '公式ホームページ始めました‼',
  '動画投稿しました‼',
];
let yt_titles = [
  '【ついに…】公式サイト作ったよ！',
  '【無意味】アジの開き閉じてみた！',
];
let yt_descs = [
  'とうとう、はるさめん公式ホームページができました！ホームページのURLはこちらです→http://harusame...',
  'アジの開きっておいしいですよね。でも、なんで開いているのか考えたことはありませんか？',
];
let article_conts = [
  `動画投稿を始めて1年が経ちました…。<br>
  ついに、公式サイトができました‼動画投稿の報告や、近況を更新していくので、楽しみにしていてください！`,
  '動画を投稿しました！ぜひ見てみてください！',
];

const timeLimit = 10 * 1000;
let startTime = 0;

export default {
  beforeMount () {
    this.first_article();
    // 初期位置
    window.scrollTo(0, 0);
  },
  mounted () {
  },
  name: 'App',
  components: {
    Article,
    Modal,
    ResultModal,
    Header,
  },
  data() {
    return {
      scroll: 0,
      height: 0,
      article_params: [{id: 1,headding: headdings[0], yt_title: yt_titles[0], yt_desc: yt_descs[0], article_cont: article_conts[0]}],
      index: 0,
      timerLeft: 10 * 1000,
      timerText: '10.00',
      timerInt: '10',
      timerFloot: '.00',
      isFinished: false,
      marginTop: 'mt-5',
      result: 0,
    }
  },
  methods: {
    scroll_start() {
      window.addEventListener('scroll', this.scroll_count);
      startTime = Date.now();
      this.updateTimer();
    },
    scroll_count() {
      // ページの位置（一番上の位置）
      this.scroll = window.pageYOffset;
      // ページ全体の高さ
      let doch = document.body.clientHeight;
      // ウィンドウの高さ
      let winh = window.innerHeight;
      // ページの最下部の位置
      let bottom = doch - winh;

      if (this.scroll === 0) this.marginTop = 'mt-5';
      else this.marginTop = ''

      if ((bottom - 50000) <= this.scroll) {
        for (let i = 0; i < 10; i++) {
          this.create_article();
        }
      }
    },

    first_article () {
      for (let i = 0; i < 10; i++) {
        this.create_article();
      }
    },

    create_article () {
      this.index++;
      this.article_params.push(
        {id: this.article_params.length+1,
        headding: headdings[this.index%headdings.length], 
        yt_title: yt_titles[this.index%yt_titles.length], 
        yt_desc: yt_descs[this.index%yt_descs.length], 
        article_cont: article_conts[this.index%article_conts.length]}
      )
    },

    // stopTimer () {
    //   window.removeEventListener('scroll', this.scroll_count);
    // },

    updateTimer () {
    this.timerLeft = startTime + timeLimit - Date.now();

    const timeoutId = setTimeout(() => {
      this.updateTimer();
    }, 10);

    // ゲームオーバー
    if (this.timerLeft < 0) {
      this.result = this.scroll;
      this.isFinished = true;
      clearTimeout(timeoutId);
      // this.stopTimer();
    }

    this.updateTimerText();
  },

    updateTimerText () {
      this.timerText = (this.timerLeft / 1000).toFixed(2);

      this.timerInt = this.reverse(this.timerText).substr(3);
      this.timerInt = this.reverse(this.timerInt);
      this.timerFloot = this.timerText.substr(-3);

      if (this.timerLeft < 0) {
      this.timerInt = '0';
      this.timerFloot = '.00';
      }
    },

    reverse (word) {
      return word.split('').reverse().join('');
    }
  },
}
</script>

<style  lang="scss">
html::-webkit-scrollbar {
  display: none;
}

.scroll {
  top: 0;
  left: 0;
}

.timer {
  top: 0;
  right: 0;
}

.container {
  width: 500px;
  margin: 0 auto;
  background: white;
}

@media screen and (max-width: 480px) {
  .container {
    width: auto;
  }
}

body {
  background: #f0eff0;
}
</style>
