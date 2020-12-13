# Оптимизация приложений nuxt.js

В статье покажу, какие основные проблемы есть при создании приложений с nuxt.js и как их решать
Мы развернем новое приложение, задеплоим его на https://vercel.com, в качестве бекэнда используем https://jsonplaceholder.typicode.com/

Про ssr и ssg - нет ни какого желания разжевывать документацию, скажу только, то что для многих остается за кадром:

Многим почему-то кажется, что используя nuxt generate на выходе получается статический сайт (что бы это не значило), 
но по факту это не совсем так (а лучше сказать совсем не так). Отличие от ssr только в том, что пререндер выполняется не 
при каждом обращении пользователя, а один раз при генерации, в остальном работает точно так же. Со всеми плюшками и 
недостатками.

## Развертывание и деплой тестового приложения
Действия ниже для тех, кто не знаком с vercel

Как написано в документации:

_Vercel - это облачная платформа для статических сайтов и Serverless функций, которая идеально подходит для вашего рабочего процесса. Он позволяет разработчикам размещать веб-сайты и веб-службы Jamstack, которые мгновенно развертываются, автоматически масштабируются и не требуют контроля, и все это без настройки._

Не знаю как в продакшине, но для теста, для "показать" заказчику, для пет-проектов — идеальная штука.


Для начала создадим новое приложение

```npx create-nuxt-app <project-name>```

Создадим для него репозиторий на github, запушим.

Заходим на https://vercel.com и регистрируемся. Попадаем на страницу https://vercel.com/dashboard
. В правом углу видим кнопку *Import Project*, жмякаем, не стесняемся, выбираем *Import a Git Repository*, 
вводим url нашего репозитория, vercel смотрит нашу репу, понимает, что у нас nuxt, предлагает нам небольшие настройки:
в данный момент нас интересует команда для билда. Т.к. мы будем использовать режим ssg, то такой командой является `npm run generate`.

Жмем красивую кнопку *deploy* и... брюки превращаются...  
в общем, надо чуть-чуть подождать (слукавила дока про "мгновенное развертывание") и готово — наше приложение увидел мир

Все эти телодвижения были нужны, что бы не только показать миру наш супер проект, но и для фишки vercel - 
при каждом комите в master, наше приложение будет пересобираться.

## Что будем делать

Вот закончили с настройкой среды, пришло время творить!

Давайте сделаем две страницы:
- главная страница, на которой отобразим список постов. Что бы не было сильно скучно, пусть они отображаются в виде 
слайдера. Для того, что бы сделать тестовый стенд более или менее похожим на настоящий проект добавим первый экран а-ля рекламный щит.
  
- страница поста. На ней выведем пост, автора и т.д...

## Поехали

### Главная страница

код в `page/index.js`
```vue
  <template>
    <div class="home-page">
      <Billboard class="home-page__block" />
      <PostsSlider class="home-page__block" />
    </div>
  </template>
  
  <script>
  import Billboard from '~/components/Billboard'
  import PostsSlider from '~/components/PostsSlider'
  
  export default {
    components: { PostsSlider, Billboard },
  }
  </script>
  
  <style lang="scss">
  .home-page {
    &__block {
      margin-bottom: 72px;
    }
  }
  </style>
```

Здесь у нас два блока Billboard и PostsSlider. Пока все как обычно.

#### Billboard

```vue
<template>
  <div class="billboard">
    <img src="/girl-4898696_1920.jpg" alt="girl" class="billboard__image" />
    <h1 class="billboard__title">{{ title }}</h1>
  </div>
</template>

<script>
export default {
  name: 'Billboard',
  async fetch() {
    // получили откуда-то из api
    this.title = await Promise.resolve('Очень, очень важный заголовок')
  },
  data() {
    return {
      title: '',
    }
  },
}
</script>

<style lang="scss">
.billboard {
  width: 100%;
  height: 100vh;

  &__image {
    width: 100%;
    height: 100%;
    object-fit: cover;
  }

  &__title {
    font-size: 6rem;
    color: #fff;
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    text-align: center;
    width: 80%;
  }
}
</style>
```

Логика работы такая: получаем с api заголовок, отображаем блок.


#### PostsSlider

```vue
<template>
  <div class="posts-slider">
    <h2 class="posts-slider__title">Наши последние посты:</h2>
    <div class="posts-slider__swiper">
        <Swiper ref="mySwiper" :options="swiperOptions" class="swiper">
          <SwiperSlide v-for="post in postsToShow" :key="post.id">
            <PostCard :post="post" />
          </SwiperSlide>
        </Swiper>
    </div>
  </div>
</template>

<script>
import PostCard from '~/components/PostCard'
export default {
  name: 'PostsSlider',
  components: { PostCard },
  async fetch() {
    this.allPosts = await this.$http.$get('/posts')
  },
  data() {
    return {
      allPosts: {
        type: Array,
        default: () => [],
      },
      swiperOptions: {
        slidesPerView: 4,
        spaceBetween: 40,
      },
    }
  },
  computed: {
    postsToShow() {
      return this.allPosts.length ? this.allPosts.slice(0, 10) : []
    },
  },
}
</script>

<style lang="scss">
.posts-slider {
  padding: 0 96px;
  max-width: 1200px;
  margin: auto;
  &__title {
    font-size: 3rem;
    margin-bottom: 40px;
  }
}
</style>
```

тут то же все тривиально: заголовок и слайдер

Получаем посты с api, т.к. бекенд не умеет в пагинацию, заводим
вычисляемое свойство, в котором берем первых десять постов.

Слайдер возьмем самый популярный https://github.com/surmon-china/vue-awesome-swiper .
Подключаем его через плагин, как написано в документации.
упс. что-то в консоли красное и не понятное... 

```
[Vue warn]: The client-side rendered virtual DOM tree is not matching server-rendered content. This is likely caused by incorrect HTML markup, for example nesting block-level elements inside <p>, or missing <tbody>. Bailing hydration and performing full client-side render.
```

Читаем доку, ага, у нас же ssr, а плагин работает только на клиенте, нужно добавить client-only
