<template>
  <div id="app" class="flex container h-screen w-full">
    <!-- side nav -->
    <div class="lg:w-1/5 border-r border-lighter px-2 lg:px-6 py-2 flex flex-col justify-between">
      <div>
        <button class="h-12 w-12 hover:bg-lightblue text-3xl rounded-full text-blue">
          <i class="fab fa-twitter"></i>
        </button>
        <div>
          <button
            v-for="tab in tabs"
            @click="id = tab.id"
            :class="`focus:outline-none hover:text-blue flex items-center py-2 px-4 hover:bg-lightblue rounded-full mr-auto mb-3 ${ id === tab.id ? 'text-blue' : ''}`"
          >
            <i :class="`${ tab.icon } text-2xl mr-4 text-left`"></i>
            <p class="text-lg font-semibold text-left hidden lg:block">{{ tab.title }}</p>
          </button>
        </div>
        <button
          class="text-white bg-blue rounded-full font-semibold focus:outline-none w-12 h-12 lg:h-auto lg:w-full p-3 hover:bg-darkblue"
        >
          <p class="hidden lg:block">Tweet</p>
          <i class="fas fa-plus lg:hidden"></i>
        </button>
      </div>
      <div class="lg:w-full relative">
        <button
          @click="dropdown = true"
          class="flex items-center w-full hover:bg-lightblue rounded-full p-2 focus:outline-none"
        >
          <img src="profile.png" class="w-10 h-10 rounded-full border border-lighter" />
          <div class="hidden lg:block ml-4">
            <p class="text-sm font-bold leading-tight">Mahmoud Diab</p>
            <p class="text-sm leading-tight">@mahmoud_w_diab</p>
          </div>
          <i class="hidden lg:block fas fa-angle-down ml-auto text-lg"></i>
        </button>
        <div
          v-if="dropdown === true"
          class="absolute bottom-0 left-0 w-64 rounded-lg shadow-md border-lightest bg-white mb-16"
        >
          <button
            @click="dropdown = false"
            class="p-3 flex items-center w-full hover:bg-lightest p-2 focus:outline-none"
          >
            <img src="profile.png" class="w-10 h-10 rounded-full border border-lighter" />
            <div class="ml-4">
              <p class="text-sm font-bold leading-tight">Mahmoud Diab</p>
              <p class="text-sm leading-tight">@mahmoud_w_diab</p>
            </div>
            <i class="fas fa-check ml-auto test-blue"></i>
          </button>
          <button
            @click="dropdown = false"
            class="w-full text-left hover:bg-lightest border-t border-lighter p-3 test-sm focus:outline-none"
          >Add an existing account</button>
          <button
            @click="dropdown = false"
            class="w-full text-left hover:bg-lightest border-t border-lighter p-3 test-sm focus:outline-none"
          >Log out @mahmoud_w_diab</button>
        </div>
      </div>
    </div>
    <!-- tweets -->
    <div class="w-full md:w-1/2 h-full overflow-y-scroll">
      <div class="px-5 py-3 border-b border-lighter flex items-center justify-between">
        <h1 class="text-xl font-bold">Home</h1>
        <i class="far fa-star text-xl text-blue"></i>
      </div>
      <div class="px-5 py-3 border-b-8 border-lighter flex">
        <div class="flex-none">
          <img src="profile.png" class="flex-none w-12 h-12 rounded-full border border-lighter" />
        </div>
        <form v-on:submit.prevent="addNewTweet" class="w-full px-4 relative">
          <textarea
            v-model="tweet.content"
            placeholder="What's up?"
            class="mt-3 pb-3 w-full focus:outline-none"
          />
          <div class="flex items-center">
            <i class="text-lg text-blue mr-4 far fa-image"></i>
            <i class="text-lg text-blue mr-4 fas fa-film"></i>
            <i class="text-lg text-blue mr-4 far fa-chart-bar"></i>
            <i class="text-lg text-blue mr-4 far fa-smile"></i>
          </div>
          <button
            type="submit"
            class="h-10 px-4 text-white font-semibold bg-blue hover:bg-darkblue focus:outline-none rounded-full absolute bottom-0 right-0"
          >Tweet</button>
        </form>
      </div>
      <div class="flex flex-col-reverse">
        <div v-for="tweet in tweets" class="w-full p-4 border-b hover:bg-lighter flex">
          <div class="flex-none mr-4">
            <img src="profile.png" class="h-12 w-12 rounded-full flex-none" />
          </div>
          <div class="w-full">
            <div class="flex items-center w-full">
              <p class="font-semibold">Mahmoud Diab</p>
              <p class="text-sm text-dark ml-2">@mahmoud_w_diab</p>
              <p class="text-sm text-dark ml-2">1 sec</p>
              <i class="fas fa-angle-down text-dark ml-auto"></i>
            </div>
            <p class="py-2">{{ tweet.content }}</p>
            <div class="flex items-center justify-between w-full">
              <div class="flex items-center text-sm text-dark">
                <i class="far fa-comment mr-3"></i>
                <p>0</p>
              </div>
              <div class="flex items-center text-sm text-dark">
                <i class="fas fa-retweet mr-3"></i>
                <p>0</p>
              </div>
              <div class="flex items-center text-sm text-dark">
                <i class="fas fa-heart mr-3"></i>
                <p>1</p>
              </div>
              <div class="flex items-center text-sm text-dark">
                <i class="fas fa-share-square mr-3"></i>
              </div>
            </div>
          </div>
        </div>
      </div>
      <div v-for="follow in following" class="w-full p-4 border-b hover:bg-lighter flex">
        <div class="flex-none mr-4">
          <img :src="`${follow.src}`" class="h-12 w-12 rounded-full flex-none" />
        </div>
        <div class="w-full">
          <div class="flex items-center w-full">
            <p class="font-semibold">{{ follow.name }}</p>
            <p class="text-sm text-dark ml-2">{{ follow.handle }}</p>
            <p class="text-sm text-dark ml-2">{{ follow.time }}</p>
            <i class="fas fa-angle-down text-dark ml-auto"></i>
          </div>
          <p class="py-2">{{ follow.tweet }}</p>
          <div class="flex items-center justify-between w-full">
            <div class="flex items-center text-sm text-dark">
              <i class="far fa-comment mr-3"></i>
              <p>{{ follow.comments }}</p>
            </div>
            <div class="flex items-center text-sm text-dark">
              <i class="fas fa-retweet mr-3"></i>
              <p>{{ follow.retweets }}</p>
            </div>
            <div class="flex items-center text-sm text-dark">
              <i class="fas fa-heart mr-3"></i>
              <p>{{ follow.like }}</p>
            </div>
            <div class="flex items-center text-sm text-dark">
              <i class="fas fa-share-square mr-3"></i>
            </div>
          </div>
        </div>
      </div>
    </div>
    <!-- trending -->
    <div
      class="md:block hidden w-1/3 h-full border-l border-lighter py-2 px-6 overflow-y-scroll relative"
    >
      <input
        class="pl-12 rounded-full w-full p-2 bg-lighter text-sm mb-4"
        placeholder="Search Twitter"
      />
      <i class="fas fa-search absolute left-0 top-0 mt-5 ml-12 text-sm text-light"></i>
      <div class="w-full rounded-lg bg-lightest">
        <div class="flex items-center justify-between p-3">
          <p class="text-lg font-bold">Trends for You</p>
          <i class="fas fa-cog text-lg text-blue"></i>
        </div>
        <button
          v-for="trend in trending"
          class="w-full flex justify-between hover:bg-lighter p-3 border-t border-lighter focus:outline-none"
        >
          <div>
            <p class="text-xs text-left leading-tight text-dark">{{ trend.top}}</p>
            <p class="font-semibold text-sm text-left leading-tight">{{ trend.title}}</p>
            <p class="text-left text-sm leading-tight text-dark">{{ trend.bottom}}</p>
          </div>
          <i class="fas fa-angle-down text-lg text-dark"></i>
        </button>
        <button
          class="p-3 w-full hover:bg-lighter text-left text-blue border-t border-lighter focus:outline-none"
        >Show More</button>
      </div>
      <div class="w-full rounded-lg bg-lightest my-4">
        <div class="p-3">
          <p class="text-lg font-bold">Who to Follow</p>
        </div>
        <button
          v-for="friend in friends"
          class="w-full flex hover:bg-lighter p-3 border-t border-lighter focus:outline-none"
        >
          <img :src="`${ friend.src }`" class="w-12 h-12 rounded-full border border-lighter" />
          <div class="hidden lg:block ml-4">
            <p class="text-sm font-bold leading-tight">{{ friend.name }}</p>
            <p class="text-sm leading-tight">{{ friend.handle }}</p>
          </div>
          <button
            class="ml-auto text-sm text-blue py-1 px-4 rounded-full border-2 border-blue focus:outline-none"
          >Follow</button>
        </button>
        <button
          class="p-3 w-full hover:bg-lighter text-left text-blue border-t border-lighter focus:outline-none"
        >Show More</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "app",
  components: {},
  data() {
    return {
      tabs: [
        { icon: "fas fa-home", title: "Home", id: "home" },
        { icon: "fas fa-hashtag", title: "Explore", id: "explore" },
        { icon: "far fa-bell", title: "Notifications", id: "notifications" },
        { icon: "far fa-envelope", title: "Messages", id: "messages" },
        { icon: "far fa-bookmark", title: "Bookmarks", id: "bookmarks" },
        { icon: "fas fa-clipboard-list", title: "Lists", id: "lists" },
        { icon: "far fa-user", title: "Profile", id: "profile" },
        { icon: "fas fa-ellipsis-h", title: "More", id: "more" }
      ],
      id: "home",
      dropdown: false,
      trending: [
        {
          top: "Trending in PS",
          title: "#Palestine",
          bottom: "Trending with: #Gaza"
        },
        { top: "News", title: "Palestinians", bottom: "135K Tweets" },
        { top: "News", title: "Gaza", bottom: "40k tweets" },
        { top: "Trending in JD", title: "Corona", bottom: "40k tweets" },
        { top: "Breaking", title: "#corona_outbreak", bottom: "25.4k tweets" }
      ],
      friends: [
        { src: "deek.jpeg", name: "كتابات ديك الجن", handle: "@deekblog" },
        { src: "hosni.jpg", name: "Mustafa Hosny", handle: "@MustafaHosny" },
        { src: "shihab.jpg", name: "وكالة شهاب", handle: "@ShehabAgency" }
      ],
      following: [
        {
          src: "elon.jpg",
          name: "Elon Musk",
          handle: "@elonmusk",
          time: "20 min",
          tweet: "Das baby kann noch keinen löffel benutzen",
          comments: "3.3K",
          retweets: "6.3K",
          like: "106.9K"
        },
        {
          src: "kevin.jpg",
          name: "Kevin Hart",
          handle: "@miniRock",
          time: "55 min",
          tweet: "Should me and the rock do another sub-par movie together????",
          comments: "2K",
          retweets: "50",
          like: "20K"
        },
        {
          src: "khaled.jpg",
          name: "Khaled ElAhmad",
          handle: "@Shusmo",
          time: "1.4 hr",
          tweet:
            "متى بتعرف انه المجتمع عنده خلل ببوصلة القيم لما تشوفه مشغول بحفلة مسبح أكتر من جريمة قتل صبية من قبل والدها وعائلتها بشكل وحشي",
          comments: "2",
          retweets: "8",
          like: "49"
        },
        {
          src: "elon.jpg",
          name: "Elon Musk",
          handle: "@teslaBoy",
          time: "1.4 hr",
          tweet: "Just did something crazyyyyyyy",
          comments: "100K",
          retweets: "14K",
          like: "52K"
        }
      ],
      tweets: [{ content: "Web development is so cool!" }],
      tweet: { content: "" }
    };
  },
  methods: {
    addNewTweet() {
      let newTweet = {
        content: this.tweet.content
      };
      this.tweets.push(newTweet);
    }
  }
};
</script>

<style>
</style>