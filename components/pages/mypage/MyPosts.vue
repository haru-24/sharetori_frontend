<template>
  <div>
    <div v-if="$mq === 'lg'">
      <div v-if="!user_post_info_datas">ローディング中</div>
      <div v-else>
        <div
          v-for="(user_post_info_data, index) in user_post_info_datas"
          :key="index"
        >
          <nuxt-link
            tag="div"
            :to="{ path: 'information/page?id=' + user_post_info_data.id }"
            class="flex justify-center cursor-pointer mt-5"
          >
            <div
              class="
                bg-gray-200
                w-5/6
                h-32
                rounded-md
                shadow-md
                flex
                justify-between
              "
            >
              <div class="mt-3 w-1/3">
                <span class="ml-3">{{ user_post_info_data.posted_at }}</span>
                <span class="ml-3">{{ user_post_info_data.prefecture }}</span>
                <span class="ml-3">{{ user_post_info_data.post_user }}</span>
                <p class="text-3xl font-semibold ml-3 mt-7">
                  {{ user_post_info_data.place_name }}
                </p>
              </div>
              <div class="mt-5 w-1/3">
                <p>住所</p>
                <p class="ml-3">{{ user_post_info_data.post_number }}</p>
                <p class="ml-3">{{ user_post_info_data.address }}</p>
              </div>
              <div class="flex items-end mb-3">
                <div class="inline-block">
                  <div class="mr-3">
                    <span class="material-icons text-3xl">
                      favorite_border
                    </span>
                    <span class="text-2xl font-medium"
                      >{{ user_post_info_data.favorites }}
                    </span>
                  </div>
                </div>
                <div class="bg-white h-24 w-40 mr-3 inline-block">
                  <img
                    v-if="user_post_info_data.image"
                    :src="user_post_info_data.image"
                    class="h-24 w-40 mr-3 inline-block"
                  />
                  <div
                    v-else
                    class="
                      bg-white
                      h-24
                      w-40
                      mr-3
                      inline-block
                      flex
                      items-center
                      text-center
                    "
                  >
                    <p class="ml-12">画像なし</p>
                  </div>
                </div>
              </div>
            </div>
          </nuxt-link>
          <div class="flex justify-center">
            <div class="w-5/6 flex justify-end">
              <button
                class="mr-3 text-lg text-red-600"
                @click="
                  deleteInfoBtnClick(
                    user_post_info_data.id,
                    user_post_info_data.img_original_url,
                    user_post_info_data.user_id
                  )
                "
              >
                削除
              </button>

              <nuxt-link
                tag="button"
                :to="{ path: '/edit', query: { id: user_post_info_data.id } }"
                class="mr-5 text-lg"
                >編集</nuxt-link
              >
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- レスポンシブ -->
    <div v-if="$mq === 'sm'">
      <div v-if="!user_post_info_datas">ローディング中</div>
      <div v-else>
        <div
          v-for="(user_post_info_data, index) in user_post_info_datas"
          :key="index"
        >
          <div class="flex justify-center">
            <nuxt-link
              tag="div"
              :to="{ path: 'information/page?id=' + user_post_info_data.id }"
              class="
                cursor-pointer
                bg-gray-100
                w-11/12
                h-20
                rounded-md
                shadow-md
                flex
                justify-between
                transform
                motion-reduce:transform-none
                hover:-translate-y-1 hover:scale-103
                transition
                ease-in-out
                duration-300
                mt-3
              "
            >
              <div class="w-3/5 mt-1">
                <p class="font-semibold ml-3 text-xs mt-1">
                  {{ user_post_info_data.place_name }}
                </p>
                <p class="ml-3 text-xs">
                  {{ user_post_info_data.post_number }}
                </p>
                <p class="ml-3 text-xs">{{ user_post_info_data.address }}</p>
              </div>

              <div class="flex items-end mb-2 w-2/5">
                <div class="inline-block mr-1">
                  <div class="">
                    <span class="material-icons"> favorite_border </span>
                    <span class="font-medium">{{
                      user_post_info_data.favorites
                    }}</span>
                  </div>
                </div>
                <img
                  v-if="user_post_info_data.image"
                  :src="user_post_info_data.image"
                  class="h-16 w-24 mr-1 inline-block"
                />
                <div
                  v-else
                  class="
                    bg-white
                    h-16
                    w-24
                    mr-1
                    inline-block
                    flex
                    items-center
                    text-center
                  "
                >
                  <p class="text-xs ml-6">画像なし</p>
                </div>
              </div>
            </nuxt-link>
          </div>

          <div class="flex justify-end mb-2">
            <button
              class="mr-2 text-red-600"
              @click="
                deleteInfoBtnClick(
                  user_post_info_data.id,
                  user_post_info_data.img_original_url,
                  user_post_info_data.user_id
                )
              "
            >
              削除
            </button>

            <nuxt-link
              tag="button"
              :to="{ path: '/edit', query: { id: user_post_info_data.id } }"
              class="mr-5"
              >編集</nuxt-link
            >
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, PropType } from '@nuxtjs/composition-api'
import { Infomation } from '@/types/types'
import { deleteInformation, deletePageFavorite } from '@/api/delete'
import { deleteStorageData } from '@/api/firebaseStorage'

export default defineComponent({
  props: {
    user_post_info_datas: {
      type: Array as PropType<Infomation[]>,
    },
  },
  setup(_props, context) {
    const deleteInfoBtnClick = async (
      id: number,
      imgUrl: string,
      userId: number
    ) => {
      if (confirm('投稿を削除しますか？')) {
        try {
          await deletePageFavorite(id)
          await deleteStorageData(imgUrl)
          await deleteInformation(id)
          context.emit('refresh_my_post_data', userId)
        } catch (err) {
          console.log(err)
          window.alert('削除に失敗しました')
        }
      }
    }
    return {
      deleteInfoBtnClick,
    }
  },
})
</script>
