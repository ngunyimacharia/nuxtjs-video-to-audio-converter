<template>
  <div class="bg-white py-2">
    <div class="relative sm:py-2">
      <div aria-hidden="true" class="hidden sm:block">
        <div class="absolute inset-y-0 left-0 w-1/2 bg-gray-50 rounded-r-3xl" />
        <svg
          class="absolute top-8 left-1/2 -ml-3"
          width="404"
          height="392"
          fill="none"
          viewBox="0 0 404 392"
        >
          <defs>
            <pattern
              id="8228f071-bcee-4ec8-905a-2a059a2cc4fb"
              x="0"
              y="0"
              width="20"
              height="20"
              patternUnits="userSpaceOnUse"
            >
              <rect
                x="0"
                y="0"
                width="4"
                height="4"
                class="text-gray-200"
                fill="currentColor"
              />
            </pattern>
          </defs>
          <rect
            width="404"
            height="392"
            fill="url(#8228f071-bcee-4ec8-905a-2a059a2cc4fb)"
          />
        </svg>
      </div>
      <div
        class="mx-auto max-w-md px-4 sm:max-w-3xl sm:px-6 lg:max-w-7xl lg:px-8"
      >
        <div
          class="
            relative
            rounded-2xl
            px-6
            py-10
            bg-indigo-600
            overflow-hidden
            shadow-xl
            sm:px-12 sm:py-20
          "
        >
          <div
            aria-hidden="true"
            class="absolute inset-0 -mt-72 sm:-mt-32 md:mt-0"
          >
            <svg
              class="absolute inset-0 h-full w-full"
              preserveAspectRatio="xMidYMid slice"
              xmlns="http://www.w3.org/2000/svg"
              fill="none"
              viewBox="0 0 1463 360"
            >
              <path
                class="text-indigo-500 text-opacity-40"
                fill="currentColor"
                d="M-82.673 72l1761.849 472.086-134.327 501.315-1761.85-472.086z"
              />
              <path
                class="text-indigo-700 text-opacity-40"
                fill="currentColor"
                d="M-217.088 544.086L1544.761 72l134.327 501.316-1761.849 472.086z"
              />
            </svg>
          </div>
          <div class="relative">
            <div class="sm:text-center">
              <h2
                class="
                  text-3xl
                  font-extrabold
                  text-white
                  tracking-tight
                  sm:text-4xl
                "
              >
                Video to audio converter
              </h2>
              <p class="mt-6 mx-auto max-w-2xl text-lg text-indigo-200">
                Upload a video into the form below to obtain the corresponding
                audio track.
              </p>
            </div>
            <form
              action="#"
              class="mt-12 sm:mx-auto sm:max-w-lg sm:flex"
              @submit.prevent="submit"
            >
              <div class="min-w-0 flex-1 text-gray-900">
                <label for="file" class="sr-only">Video</label>
                <input
                  accept="video/*"
                  @change="handleFile"
                  id="file"
                  type="file"
                  class="
                    block
                    w-full
                    border border-transparent
                    rounded-md
                    px-5
                    py-3
                    text-base text-gray-900
                    placeholder-gray-500
                    shadow-sm
                    focus:outline-none
                    focus:border-transparent
                    focus:ring-2
                    focus:ring-white
                    focus:ring-offset-2
                    focus:ring-offset-indigo-600
                  "
                />
              </div>
              <div class="mt-4 sm:mt-0 sm:ml-3">
                <p
                  v-if="uploading"
                  class="text-center text-sm font-semibold text-white px-5 py-3"
                >
                  Uploading...
                </p>
                <button
                  v-else
                  type="submit"
                  class="
                    block
                    w-full
                    rounded-md
                    border border-transparent
                    px-5
                    py-3
                    bg-indigo-500
                    text-base
                    font-medium
                    text-white
                    shadow
                    hover:bg-indigo-400
                    focus:outline-none
                    focus:ring-2
                    focus:ring-white
                    focus:ring-offset-2
                    focus:ring-offset-indigo-600
                    sm:px-10
                  "
                >
                  Upload
                </button>
              </div>
            </form>
          </div>
        </div>
      </div>
    </div>
    <div class="m-8 text-center" v-if="cloudinaryVideo">
      <audio controls autoplay class="mx-auto">
        <source :src="audioUrl" type="audio/mpeg" />
        Your browser does not support the audio element.
      </audio>
      <a
        :href="audioUrl"
        target="_blank"
        class="
          m-4
          inline-flex
          items-center
          px-3
          py-2
          border border-transparent
          text-sm
          leading-4
          font-medium
          rounded-md
          text-indigo-700
          bg-indigo-100
          hover:bg-indigo-200
          focus:outline-none
          focus:ring-2
          focus:ring-offset-2
          focus:ring-indigo-500
        "
      >
        Download
      </a>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      uploading: false,
      video: null,
      cloudinaryVideo: null,
    };
  },
  computed: {
    audioUrl() {
      return this.cloudinaryVideo
        ? this.$cloudinary.video.url(this.cloudinaryVideo.public_id, {
            format: "mp3",
          })
        : "";
    },
  },
  methods: {
    async handleFile(e) {
      this.video = e.target.files[0];
    },
    async readData(f) {
      return new Promise((resolve) => {
        const reader = new FileReader();
        reader.onloadend = () => resolve(reader.result);
        reader.readAsDataURL(f);
      });
    },
    async submit() {
      this.uploading = true;
      const videoData = await this.readData(this.video);
      this.cloudinaryVideo = await this.$cloudinary.upload(videoData, {
        upload_preset: "default-preset",
        folder: "nuxtjs-video-to-audio-converter",
      });
      console.log(this.cloudinaryVideo);
      this.uploading = false;
    },
  },
};
</script>