# @maticonoffice/document-editor-vue

[RU](README.md) | [EN](README.en.md)

Этот репозиторий содержит Vue.js-компонент MaticonOffice Docs, который интегрирует [MATICONOFFICE Document Server](https://github.com/MaticonOffice/DocumentServer) в проекты на [Vue.js](https://vuejs.org/).

**Обратите внимание:** перед работой с этим компонентом нужно установить MaticonOffice Docs. Для этого можно использовать [Docker](https://github.com/MaticonOffice/Docker-DocumentServer) (рекомендуется).

## Предварительные требования
Для этой процедуры требуется [Node.js и npm](https://nodejs.org/en).

## Создание демонстрационного Vue.js-приложения с редактором MATICONOFFICE Docs
Эта процедура создаёт [базовое Vue.js-приложение](https://cli.vuejs.org/guide/creating-a-project.html#vue-create) и устанавливает в него редактор MATICONOFFICE Docs.

1. Откройте командную строку или терминал и создайте проект Vue.js 3.x с именем *maticonoffice-vue-demo* с помощью [Create Vue Tool](https://github.com/vuejs/create-vue):

```
npm create vue@3
```

2. Перейдите в созданный каталог:

```
cd maticonoffice-vue-demo
```

3. Установите Vue.js-компонент MATICONOFFICE Docs из **npm** и сохраните его в файле *package.json* с параметром *--save*:

```
npm install --save @maticonoffice/document-editor-vue
```

4. Откройте файл *./src/App.vue* в проекте *maticonoffice-vue-demo* и замените его содержимое следующим кодом:

```
<template>
    <DocumentEditor 
        id="docEditor" 
        documentServerUrl="http://documentserver/"
        :config="config"
        :onLoadComponentError="onLoadComponentError"
    /> 
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import { DocumentEditor } from "@maticonoffice/document-editor-vue";

export default defineComponent({
    name: 'ExampleComponent',
    components: {
        DocumentEditor
    },
    data() {
        const onDocumentReady = () => {
            console.log("Document is loaded");
        };

        return {
            config: {
                document: {
                    fileType: "docx",
                    key: "Khirz6zTPdfd7",
                    title: "Example Document Title.docx",
                    url: "https://example.com/url-to-example-document.docx"
                },
                documentType: "word",
                editorConfig: {
                    callbackUrl: "https://example.com/url-to-callback.ashx"
                },
                events: {
                    onDocumentReady: onDocumentReady
                }
            }
        }
    },
    methods: {
        onLoadComponentError (errorCode, errorDescription) {
            switch(errorCode) {
                case -1: // Unknown error loading component
                    console.log(errorDescription);
                    break;

                case -2: // Error load DocsAPI from http://documentserver/
                    console.log(errorDescription);
                    break;

                case -3: // DocsAPI is not defined
                    console.log(errorDescription);
                    break;
            }
        }
    },
});
</script>
```

Замените следующие строки своими данными:
* **"http://documentserver/"** — замените URL вашего сервера;
* **"https://example.com/url-to-example-document.docx"** — замените URL вашего файла;
* **"https://example.com/url-to-callback.ashx"** — замените URL callback-обработчика (это необходимо для работы сохранения).

5. Проверьте приложение с помощью development-сервера Vue:

* Чтобы запустить development-сервер, перейдите в каталог *maticonoffice-vue-demo* и выполните:

```
npm run dev
```

* Чтобы остановить development-сервер, выберите командную строку или терминал и нажмите *Ctrl+C*.

## Развёртывание демонстрационного Vue.js-приложения

Самый простой способ развернуть приложение в production-окружении — установить [serve](https://github.com/vercel/serve) и создать статический сервер:

1. Установите пакет *serve* глобально:

```
npm install -g serve
```

2. Запустите статический сайт на порту 3000:

```
serve -s build
```

Другой порт можно указать с помощью флагов *-l* или *--listen*:

```
serve -s build -l 4000
```

3. Чтобы обслуживать папку проекта, перейдите в неё и выполните команду *serve*:

```
cd maticonoffice-react-demo
serve
```

Теперь приложение можно развернуть на созданном сервере:

1. Перейдите в каталог *maticonoffice-vue-demo* и выполните:

```
npm run build
```

Будет создан каталог *dist* с production-сборкой приложения.

2. Скопируйте содержимое каталога *maticonoffice-vue-demo/dist* в корневой каталог веб-сервера (в папку *maticonoffice-react-demo*).

Приложение будет развёрнуто на веб-сервере (*http://localhost:3000* по умолчанию).

## API
### Props
| Имя | Тип | По умолчанию | Обязательный | Описание |
| ------------- | ------------- | ------------- | ------------- | ------------- |
| `id` | string | null | yes | Уникальный идентификатор компонента. |
| `documentServerUrl` | string | null | yes | Адрес MATICONOFFICE Document Server. |
| `shardkey` | string \| boolean | true | no | Строковый или логический параметр, необходимый для балансировки нагрузки при совместном редактировании: все пользователи, редактирующие один документ, обслуживаются одним сервером. [Shard key](https://api.maticonoffice.ru/docs/docs-api/get-started/how-it-works/#shard-key) |
| `config` | object | null | yes | Общий объект конфигурации для открытия файла с token. [Config API](https://api.maticonoffice.ru/docs/docs-api/usage-api/config/) |
| `onLoadComponentError` | (errorCode: number, errorDescription: string) => void | null | no | Функция, вызываемая при ошибке загрузки компонента. |

## Storybook

Измените адрес Document Server в файле *config/default.json*:
```
"documentServerUrl": "http://documentserver/"
```

### Собрать Storybook:
```
npm run build-storybook
```
### Запустить Storybook:
```
npm run storybook
```

## Разработка

### Клонируйте проект из GitHub-репозитория:
```
git clone https://github.com/MaticonOffice/document-editor-vue
```
### Установите зависимости проекта:
```
npm install
```
### Запустите тесты компонента:
```
npm run test
```
### Соберите проект:
```
npm run build
```
### Создайте пакет:
```
npm pack
```

## Обратная связь и поддержка

Если у вас возникли проблемы, вопросы или предложения по Vue-компоненту MaticonOffice Document Server, обратитесь к разделу [Issues](https://github.com/MaticonOffice/document-editor-vue/issues).

Официальный сайт проекта: [www.maticonoffice.ru](https://www.maticonoffice.ru/).

Форум поддержки: [forum.maticonoffice.ru](https://forum.maticonoffice.ru/).
