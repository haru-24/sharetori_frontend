<template>
  <div>
    <div>
      <div v-if="$mq === 'lg'" class="ml-10">
        <div class="mr-7 font-bold">
          <p class="inline mr-10 text-2xl">コメント</p>
          <div v-show="$auth.loggedIn" class="inline">
            <input
              v-model="inputReview"
              type="text"
              class="h-7 w-56 border-2 boder-gray-200 text-sm inline"
            />
            <button
              class="
                bg-gray-500
                hover:bg-gray-700
                text-white
                px-2
                rounded
                inline
                text-lg
              "
              @click="clickAddReviewBtn"
            >
              追加
            </button>
          </div>
        </div>

        <div
          v-for="(reviewData, index) in reviewDatas"
          :key="index"
          class="text-lg ml-20 mt-3"
        >
          <p class="inline-block mr-3">{{ reviewData.reviewed_at }}</p>
          <p class="inline-block mr-3">{{ reviewData.user_birth_place }}</p>
          <p class="inline-block mr-3 w-28">{{ reviewData.user }}</p>
          <div class="inline-block ml-10">
            <p>{{ reviewData.review }}</p>
          </div>

          <div
            v-if="$auth.user && $auth.user.id === reviewData.user_id"
            class="-mt-3"
          >
            <button
              class="text-xs text-red-600 hover:text-red-900"
              @click="commentDeleteBtnClick(reviewData.id, index)"
            >
              コメントの削除
            </button>
          </div>
        </div>
      </div>
      <!-- レスポンシブ -->
      <div v-if="$mq === 'sm'">
        <div class="ml-2 mt-4">
          <div class="font-bold">
            <p class="inline mr-2 text-base">コメント</p>
            <div v-show="$auth.loggedIn" class="inline">
              <input
                v-model="inputReview"
                type="text"
                class="h-5 w-56 border-2 boder-gray-200 text-xs inline"
              />
              <button
                class="
                  bg-gray-500
                  hover:bg-gray-700
                  text-white
                  px-1
                  rounded
                  inline
                  text-sm
                "
                @click="clickAddReviewBtn"
              >
                追加
              </button>
            </div>
          </div>

          <div
            v-for="(reviewData, index) in reviewDatas"
            :key="index"
            class="text-xs ml-2 mt-3"
          >
            <p class="inline-block mr-1">{{ reviewData.reviewed_at }}</p>
            <p class="inline-block mr-1">{{ reviewData.user_birth_place }}</p>
            <p class="inline-block mr-1">{{ reviewData.user }}</p>
            <div
              v-if="$auth.user && $auth.user.id === reviewData.user_id"
              class="inline-block flex justify-end -mt-4 mr-2"
            >
              <button
                class="text-xs text-red-600 hover:text-red-900"
                @click="commentDeleteBtnClick(reviewData.id, index)"
              >
                削除
              </button>
            </div>
            <div class="ml-2 mt-1">
              <p>{{ reviewData.review }}</p>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref, useStore } from '@nuxtjs/composition-api'
import { getAllReviewData } from '../../../api/get'
import { postReviewData } from '../../../api/post'
import { ReviewData } from '~/types/types'
import { deleteReview } from '@/api/delete'

export default defineComponent({
  props: ['pageid'],
  setup(props) {
    const reviewDatas = ref<ReviewData[]>()
    const inputReview = ref<string>('')

    getAllReviewData(props.pageid).then((result) => {
      reviewDatas.value = result
      console.log(reviewDatas.value)
    })

    const storeUserData = useStore().$auth.$state.user

    const clickAddReviewBtn = () => {
      postReviewData(
        inputReview.value,
        props.pageid,
        storeUserData.id,
        storeUserData.user_name,
        storeUserData.birth_place
      ).then((result) => {
        reviewDatas.value?.unshift(result)
        inputReview.value = ''
      })
    }

    const commentDeleteBtnClick = (reviewId: number, index: number) => {
      if (confirm('コメントを削除しますか？')) {
        deleteReview(reviewId).then(() => {
          reviewDatas.value?.splice(index, 1)
        })
      }
    }

    return {
      inputReview,
      clickAddReviewBtn,
      reviewDatas,
      commentDeleteBtnClick,
    }
  },
})
</script>
