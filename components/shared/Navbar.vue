<template>
  <div class="">
    <nav
      v-if="$mq === 'lg'"
      class="
        flex flex-col
        text-center
        sm:flex-row sm:text-left sm:justify-between
        py-4
        px-6
        shadow
        sm:items-baseline
        w-full
        bg-green-400
      "
    >
      <div class="mb-2 sm:mb-0">
        <nuxt-Link
          tag="a"
          to="/"
          class="
            text-3xl
            no-underline
            text-gray-800
            hover:text-blue-dark
            font-extrabold
            wf-nicomoji
          "
          >シェアトリ</nuxt-Link
        >

        <button
          class="
            text-lg
            no-underline
            text-grey-darkest
            hover:text-blue-dark
            ml-10
          "
          @click="onClickNavSearchBtn"
        >
          <span class="material-icons text-2xl"> search </span>探す
        </button>
        <nuxt-Link
          v-show="$auth.loggedIn"
          tag="button"
          to="/post"
          class="
            text-lg
            no-underline
            text-grey-darkest
            hover:text-blue-dark
            ml-5
          "
          ><span class="material-icons text-2xl"> assignment </span
          >投稿</nuxt-Link
        >
      </div>
      <div class="">
        <!-- 認証しているかどうかで変化 -->
        <div v-show="!$auth.loggedIn" class="inline">
          <nuxt-Link tag="button" to="/login" class="text-lg text-grey-darkest"
            >ログイン<span class="material-icons text-4xl mb-1">
              login
            </span></nuxt-Link
          >
        </div>
        <nuxt-Link v-if="$auth.loggedIn" tag="a" to="/mypage">
          <span class="font-semibold">{{ $auth.user.user_name }}</span>
          <span class="material-icons text-4xl inline mb-1">
            account_circle
          </span>
        </nuxt-Link>
      </div>
    </nav>
    <!-- レスポンシブ対応 -->
    <div v-if="$mq === 'sm'">
      <nav
        class="flex justify-between py-1 pb-2 px-6 shadow w-full bg-green-400"
      >
        <nuxt-Link
          tag="a"
          to="/"
          class="
            text-xl
            no-underline
            text-gray-800
            font-extrabold
            wf-nicomoji
            mt-1
          "
          >シェアトリ</nuxt-Link
        >
        <div class="mt-2 space-x-1">
          <button @click="onClickNavSearchBtn">
            <span class="material-icons text-3xl inline-block"> search </span>
          </button>

          <nuxt-Link v-show="$auth.loggedIn" tag="button" to="/post"
            ><span class="material-icons text-3xl">
              assignment
            </span></nuxt-Link
          >
          <nuxt-link v-show="!$auth.loggedIn" tag="button" to="/login">
            <span class="material-icons text-3xl"> login </span>
          </nuxt-link>
          <nuxt-Link v-if="$auth.loggedIn" tag="a" to="/mypage">
            <span class="material-icons text-3xl inline-block">
              account_circle
            </span>
          </nuxt-Link>
        </div>
      </nav>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from '@nuxtjs/composition-api'
export default defineComponent({
  setup(_props, context) {
    const onClickNavSearchBtn = () => {
      // サーチページにいる場合フェッチする
      if (location.pathname === '/search') {
        context.emit('all-serch-infomation')
        // クエリストリングを消すためのpush
        context.root.$router.push({
          path: '/search',
          query: { p: '1', sort: 'new_arrival' },
        })
      } else {
        // 違う場合サーチに遷移する
        context.root.$router.push({
          path: '/search',
          query: { p: '1', sort: 'new_arrival' },
        })
      }
    }
    return {
      onClickNavSearchBtn,
    }
  },
})
</script>
<style scoped>
.wf-nicomoji {
  font-family: 'Nico Moji';
}
</style>
