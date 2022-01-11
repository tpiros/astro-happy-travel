<template>
  <div>
    <div class="border-2 border-indigo-800 mb-2">
      <img :src="cldImage" />
    </div>
    <div class="flex items-center justify-between">
      <a
        class="hover:bg-indigo-500 bg-indigo-700 text-white font-bold py-2 px-4 rounded focus:outline-none focus:shadow-outline hover:cursor-pointer"
        :download="cldImage"
        :href="cldImage"
      >
        "Send"
      </a>
    </div>
  </div>
</template>

<script>
import { Cloudinary } from '@cloudinary/url-gen';
import { source } from '@cloudinary/url-gen/actions/overlay';
import { text } from '@cloudinary/url-gen/qualifiers/source';
import { TextStyle } from '@cloudinary/url-gen/qualifiers/textStyle';
import { Position } from '@cloudinary/url-gen/qualifiers/position';
import { compass } from '@cloudinary/url-gen/qualifiers/gravity';
import { scale } from '@cloudinary/url-gen/actions/resize';

import { computed } from 'vue';

export default {
  name: 'PostcardDisplay',
  props: {
    message: String,
    image: String,
  },
  setup(props) {
    const cld = new Cloudinary({
      cloud: {
        cloudName: 'tamas-demo',
      },
      url: {
        secure: true,
      },
    });

    const cldImage = computed(() => {
      const myImage = cld.image(props.image);
      if (props.message.length === 0) {
        return myImage
          .resize(scale().width(1920))
          .format('auto')
          .quality('auto')
          .toURL();
      } else {
        return myImage
          .overlay(
            source(
              text(
                props.message,
                new TextStyle('Cookie', 120).fontWeight('bold')
              ).textColor('#fff')
            ).position(
              new Position()
                .gravity(compass('north'))
                .offsetX(-800)
                .offsetY(150)
            )
          )
          .resize(scale().width(1920))
          .format('auto')
          .quality('auto')
          .toURL();
      }
    });
    return {
      cldImage,
    };
  },
};
</script>
