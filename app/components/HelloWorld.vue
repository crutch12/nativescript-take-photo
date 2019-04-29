<template>
	<Page class="page">
		<ActionBar :title="`Фото ${takenPhotos.length ? takenPhotos.length : ''}`" class="action-bar" />
    <StackLayout>
      <Button :text="textPicture" class="btn btn-primary take-button" @tap="takePicture"></Button>
      <ScrollView orientation="vertical">
        <FlexboxLayout class="photos">
          <StackLayout orientation="vertical" v-for="(photo, idx) in takenPhotos" :key="idx">
            <TextView editable="false" :text="`Время создания: ${photo.time}` "/>
            <Image :src="photo.src" class="photo"></Image>
          </StackLayout>
        </FlexboxLayout>
      </ScrollView>
    </StackLayout>
	</Page>
</template>

<script>
  import * as camera from 'nativescript-camera';
  import * as http from 'http';
  import * as moment from 'moment';

  export default {
    data () {
      return {
        takenPhotos: [],
        textPicture: 'Сделать фото',
        moment: moment,
      };
    },
    methods: {
      takePicture() {
        // See the options at https://github.com/NativeScript/nativescript-camera#using-the-options-to-take-memory-efficient-picture
        // for more information on how to customize the pictures you take.
        camera
          .takePicture({ width: 300, height: 300, keepAspectRatio: true })
          .then(imageAsset => {
            this.takenPhotos.push({
              src: imageAsset,
              time: moment().format('LT'),
            })
          })
          .catch(err => {
            console.error("Error -> " + err.message);
          });
      },
    },
  }
</script>

<style lang="scss" scoped>
  .take-button {
    margin-top: 20;
  }
  .photos {
    flex-wrap: wrap;
    padding: 20 10 20 10;
  }
  .photo {
    border-radius: 10;
    margin-bottom: 10;
    width: 100%;
  }
</style>
