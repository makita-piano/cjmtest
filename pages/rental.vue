<template>
  <v-main>
    <div class="col-12">
      <nuxt-img src="/img/CJM_studio.png" width="1024" class="width-100" alt="CJM_studio" />
    </div>
    <div class="grid-center col-12" style="padding: 2rem 0 1rem 0;background-color: #F6EDD9;text-align: center;margin-bottom: 2rem;">
      <div class="grid col-9_sm-10">
        <div class="col-3_sm-6" style="margin-bottom: 1rem">
          <a href="#c1">設備</a>
        </div>
        <div class="col-3_sm-6" style="margin-bottom: 1rem">
          <a href="#c2">料金</a>
        </div>
        <div class="col-3_sm-6" style="margin-bottom: 1rem">
          <a href="#c3">禁止事項</a>
        </div>
        <div class="col-3_sm-6" style="margin-bottom: 1rem">
          <a href="#c4">スタジオ予約</a>
        </div>
      </div>
    </div>
    <div id="main-content" class="grid-center col-12">
      <div class="grid col-9_sm-12">
        <div class="col-12">
          <p>
            CJM Tokyo 神宮の杜音楽院 （東京都渋谷区千駄ヶ谷）では、防音スタジオ(24.9m2 南西向き 天井高2500mm 天然木無垢床材) の貸出を行なっております。<br> 個人の練習や室内楽の合わせなどはもちろん、録音・録画スタジオとしてもご利用いただけます。オンライン審査のための演奏収録、コンクール直前の練習など、様々な用途にぜひお使いください。
          </p>
        </div>

        <h2 id="c1" class="col-12">設備</h2>
        <div class="grid col-12" style="padding: .5rem 0">
          <div v-for="(e,i) in room_imgs" v-bind:key="e.title" class="grid col-6" :class="calc_class(i)">
            <div class="grid-top col-12">
              <div class="col-12" style="margin-bottom: 1rem;">
                <img :src="e.img.url" class="width-100" alt="ins-image">
              </div>
              <div class="col-12">
                <h3>{{ e.title}}</h3>
              </div>
              <div class="col-12">
                <p>
                  {{ e.txt }}
                </p>
              </div>
            </div>
          </div>

          <div class="button" v-on:click="see_more" v-show="room_imgs_other == undefined" style="cursor: pointer">
            <span>See More</span>
          </div>

          <div v-if="room_imgs_other != undefined" v-for="(e,i) in room_imgs_other" v-bind:key="e.title" class="grid col-6" :class="calc_class(i)">
            <div class="grid-top col-12">
              <div class="col-12" style="margin-bottom: 1rem;">
                <img :src="e.img.url" class="width-100" alt="ins-image">
              </div>
              <div class="col-12">
                <h3>{{ e.title}}</h3>
              </div>
              <div class="col-12">
                <p>
                  {{ e.txt }}
                </p>
              </div>
            </div>
          </div>
        </div>

        <h2 id="c2" class="col-12" style="margin-top: 4rem">料金</h2>
        <h3 class="col-12"><p>基本料金　</p><span style="font-size: 1rem">※1時間あたり / YAMAHA C3型 グランドピアノ 使用可能</span></h3>
        <ul class="col-12 pre_list" style="margin-bottom: 3rem">
          <li><p>・平日 午前（9:00-13:00）　	￥2,000</p></li>
          <li><p>・平日 午後（13:00-17:00）　	￥2,500</p></li>
          <li><p>・平日 夜間（17:00-23:00）　	￥3,000</p></li><br>
          <li><p>・土日祝日 （終日）　	￥3,000</p></li>
        </ul>
        <h3 class="col-12">オプション　<span style="font-size: 1rem">※１時間あたり</span></h3>
        <ul class="col-12" style="margin-bottom: 3rem">
          <li><p>・スタインウェイ使用料　￥1,000</p></li>
        </ul>

        <h3 class="col-12">会員特典　<span style="font-size: 1rem">※オプション料金には適用されません。　</span></h3>
        <ul class="col-12">
          <li><p>・スタジオ利用料　20%OFF</p></li>
        </ul>

        <h2 id="c3" class="col-12" style="margin-top: 5rem">禁止事項</h2>
        <ul class="col-12 pre_list" style="margin-bottom: .5rem">
          <li><p>・音楽以外の目的での使用</p></li>
          <li><p>・打楽器の演奏（金管楽器、サックスの演奏は、ご相談ください）</p></li>
          <li><p>・建物および施設内での喫煙</p></li>
          <li><p>・無断で空調を設定すること</p></li>
          <li><p>・無断で窓を開放すること</p></li>
          <li><p>・演奏中に限らず、入退室時以外に扉を開放すること</p></li>
        </ul>

        <ul class="col-12 pre_list">
          <li><p>※スタジオ利用は、音楽院のレッスン等がない空き時間に限られます。状況により、空き時間が変わる可能性がございますことを、ご了承ください。</p></li>
          <li><p>※レッスン室としてのご利用をご検討の方はご相談ください。</p></li>
          <li><p>※飲食は可能ですが、ゴミはお持ち帰りください。</p></li>
        </ul>

        <h2 id="c4" class="col-12" style="margin-top: 5rem">スタジオ予約</h2>
        <div class="col-12 color-blue" style="margin-bottom: 2rem">
          <RouterLink to="/contact">スタジオのご予約はこちらから</RouterLink>
        </div>
      </div>
    </div>
  </v-main>
</template>

<script>
  import axios from 'axios'
  export default {
    name: "rental",
    layout: 'default',

    head() {
      return {
        title: "スタジオ貸出｜神宮の杜音楽院",
        meta: [
          { hid: 'description', name: 'description', content: '神宮の杜音楽院のスタジオ貸出ページです。CJM Tokyo 神宮の杜音楽院は、東京藝術大学卒業などの一流の学歴と留学経験を持つ、国内最高レベルの権威あるクラシック音楽家による、ピアノレッスンを中心とした音楽教育を行なっております。個人レッスンのみならず、音楽理論やソルフェージュのクラス、上級者のための演奏法講座、海外より招聘する世界クラスの音楽家による特別レッスンなど、広く展開していきます。' },
          { hid: 'og:description', property: 'og:description', content: '神宮の杜音楽院のスタジオ貸出ページです。CJM Tokyo 神宮の杜音楽院は、東京藝術大学卒業などの一流の学歴と留学経験を持つ、国内最高レベルの権威あるクラシック音楽家による、ピアノレッスンを中心とした音楽教育を行なっております。個人レッスンのみならず、音楽理論やソルフェージュのクラス、上級者のための演奏法講座、海外より招聘する世界クラスの音楽家による特別レッスンなど、広く展開していきます。' },
          { hid: 'og:title', property: 'og:title', content: 'スタジオ貸出｜神宮の杜音楽院' }
        ]
      }
    },

    data () {
      return {
        room_imgs: undefined,
        room_imgs_other: undefined,
      }
    },

    created(){
      axios
        .get(process.env.VUE_APP_MICROCMS_URL + "/room_img?fields=img,title,txt&limit=4",{
          headers: { "X-MICROCMS-API-KEY": process.env.VUE_APP_MICROCMS_KEY},
          data: {}
      })
        .then(v => {
          this.room_imgs = v.data.contents;
      });
    },
    methods: {
      calc_class: function(index) {
        return "room-img-" + index%2
      },
      see_more: function() {
        axios
          .get(process.env.VUE_APP_MICROCMS_URL + "/room_img?fields=img,title,txt&offset=4",{
            headers: { "X-MICROCMS-API-KEY": process.env.VUE_APP_MICROCMS_KEY},
            data: {}
        })
        .then(v => {
          this.room_imgs_other = v.data.contents;
        });
      }
    }
  }
</script>

<style>
.button {
  display: block;
  position: relative;
  background: #bbb;
  width: 80%;
  padding: 1.5rem;
  border-radius: .6rem;
  color: #fff;
  text-align: center;
  text-decoration: none;
  font-weight: bold;
  letter-spacing: .1rem;
  transition: .5s;
  margin: auto;
}
.button:before {
  content: "";
  position: absolute;
  top: 80%;
  right: calc(50% + 3px);
  width: 10px;
  height: 2px;
  background: #fff;
  transform: rotate(45deg);
}
.button:after {
  content: "";
  position: absolute;
  top: 80%;
  right: calc(50% - 3px);
  width: 10px;
  height: 2px;
  background: #fff;
  transform: rotate(-45deg);
}

#body .room-img-0 {
  border-right: 1px solid #999;
  padding-right: 1rem;
  margin: 1rem 0;
}
#body .room-img-1 {
  padding-left: 1rem;
  margin: 1rem 0;
}

@media (min-width: 769px) {
  #body .room-img-0 {
    padding-right: 2rem;
  }
  #body .room-img-1 {
    padding-left: 2rem;
  }
}
</style>
