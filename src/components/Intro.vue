<template>
    <section class="main">
        <div class="main__content container">
            <div class="main__content-swiper">
                <swiper
                    :modules="modules"
                    :effect="'creative'"
                    :creativeEffect="{
                        prev: {
                            shadow: true,
                            translate: ['-120%', 0, -500],
                        },
                        next: {
                            shadow: true,
                            translate: ['120%', 0, -500],
                        },
                        }"
                    :spaceBetween="30"
                    :centeredSlides="true"
                    :autoplay="{
                      delay: 1500,
                      disableOnInteraction: false,
                    }"
                    class="mySwiper">
                    <swiper-slide>
                        <div class="main__swiper-body">
                            <div class="main__body-info">
                                <h2 class="main__info-title">Новая <span>коллекция</span></h2>
                                <a href="#" class="main__info-btn">Смотреть Новинки ></a>
                            </div>
                        </div>
                        <div class="main__swiper-bg">
                            <img src="/src/img/main-bg.png" alt="" class="main__bg-img">
                        </div>
                    </swiper-slide>
                    <swiper-slide>
                        <div class="main__swiper-body">
                            <div class="main__body-info">
                                <h2 class="main__info-title">Новая <span>коллекция</span></h2>
                                <a href="#" class="main__info-btn">Смотреть Новинки ></a>
                            </div>
                        </div>
                        <div class="main__swiper-bg">
                            <img src="/src/img/main-bg.png" alt="" class="main__bg-img">
                        </div>
                    </swiper-slide>
                </swiper>
            </div>
        </div>
    </section>
</template>
<script>
  import { Swiper, SwiperSlide } from 'swiper/vue';
  import 'swiper/css';
  import 'swiper/css/pagination';
  import 'swiper/css/navigation';
  import { Autoplay, EffectCreative  } from 'swiper/modules';

  export default {
    components: {
      Swiper,
      SwiperSlide,
    },
    setup() {
      return {
        modules: [Autoplay, EffectCreative],
      };
    },
  };
</script>
