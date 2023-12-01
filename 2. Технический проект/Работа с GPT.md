# Ссылки
[Quickstart](https://platform.openai.com/docs/quickstart?context=node)
[Токенайзер](https://platform.openai.com/tokenizer) перевод текста в предварительные токены 
[Playground](https://platform.openai.com/playground?mode=chat)- песочница для просмотра запросов и их ответов.
[API reference](https://platform.openai.com/docs/api-reference/introduction) - дока по API
# Модели

Модель | Описание
-------- | --------
[GPT-4 and GPT-4 Turbo](https://platform.openai.com/docs/models/gpt-4-and-gpt-4-turbo)|Набор моделей, которые улучшают GPT-3.5 и могут понимать, а также генерировать естественный язык или код.
[GPT-3.5](https://platform.openai.com/docs/models/gpt-3-5)| Набор моделей, которые улучшают GPT-3 и могут понимать, а также генерировать естественный язык или код.
[DALL·E](https://platform.openai.com/docs/models/dall-e)|Модель, которая может генерировать и редактировать изображения с помощью подсказки на естественном языке.
[TTS](https://platform.openai.com/docs/models/tts)|Набор моделей, которые могут преобразовывать текст в естественно звучащую речь.
[Whisper](https://platform.openai.com/docs/models/whisper)|Модель, которая может конвертировать аудио в текст
[Embeddings](https://platform.openai.com/docs/models/embeddings)|Набор моделей, способных преобразовывать текст в числовую форму.
[Moderation](https://platform.openai.com/docs/models/moderation)|Точно настроенная модель, которая может определить, является ли текст конфиденциальным или небезопасным.
[GPT base](https://platform.openai.com/docs/models/gpt-base)|Набор моделей без инструкций, которые могут понимать, а также генерировать естественный язык или код.
[GPT-3](https://platform.openai.com/docs/models/gpt-3)Legacy| Набор моделей, которые могут понимать и генерировать естественный язык.
[Deprecated](https://platform.openai.com/docs/deprecations)| Полный список моделей, которые устарели, а также предлагаемые замены.

## Модели для ресерчеров

Модель/API | Описание
-------- | --------
text-davinci-003|модель InstructGPT. Обучение с подкреплением с помощью моделей вознаграждения, обученных на основе сравнений, проведенных людьми.
gpt-3.5-turbo | Улучшение text-davinci-003 оптимизированная для чата
gpt-3.5-turbo-1106 | Обновленый GPT 3.5 Turbo Новейшая модель GPT-3.5 Turbo с улучшенным выполнением инструкций, режимом JSON, воспроизводимыми выходными данными, параллельным вызовом функций и многим другим. Возвращает максимум 4096 выходных токенов. Принимает в контекст 16,385 токенов



# Локальное разворачивание
## Node.js
Создаёте пустой проект посредством **npm install node**
**npm install --save openai**
В _package.json_ добавляете "type": "module"
В файле js прописываете 
```js
import OpenAI from "openai";

  

const openai = new OpenAI({apiKey : 'YOUR_API_KEY'});

  

async function main() {

  const completion = await openai.chat.completions.create({

    messages: [{ role: "system", content: "You are a helpful assistant." }],

    model: "gpt-3.5-turbo",

  });

  

  console.log(completion.choices[0]);

}

  

main();
```
Запускаете посредством терминала **node openai_test.js** 
В ответ выдаст в терминале:
```js
{
  index: 0,
  message: { role: 'assistant', content: 'How may I assist you today?' },
  finish_reason: 'stop'
}
```


# Работа с запросами и ответами
## Assistants
## Threads 
## Messages
## Runs
## Completions (отрубят 4 января)
### Request body

```js
{
    model: "gpt-3.5-turbo-instruct",
    prompt: "Say this is a test.",
    max_tokens: 7,
    temperature: 0,
  }
```
* **model** *required* - модель нужная нам из [списка моделей онлайн](https://platform.openai.com/docs/api-reference/models/list) и [[#Модели]]
* **prompt** *required* - запрос в виде текста 
* ***max_tokens** *default = 16*- ограничение на использование токенов
* **temperature** *default = 1* - 
	*"разброс" или же точность выборки, используйте значения между 0 и 2. Большие значения как 0.8 сделают выходные данные более случайными, меньшие как 0.2 сделают их более целенаправленными и детерминированными.
* **top_p**  *optional* -
	альтернатива для температуры, выборка ядра. Модель учитывает результаты токенов с вероятной массой. Значит что 0.1 что учитываются только токены, составляющие верхние 10% вероятности массы. **ИСПОЛЬЗОВАТЬ ТОЛЬКО ОДНО ИЗ НИХ!**
* **messages** *optional* - сообщения с параметрами *role* и *content* . Prompt то же самое, только сразу выдаёт роль *user*
* **seed** *optional* - 
	*Если указано,  система приложит все усилия для детерминированной выборки, чтобы повторные запросы с одним и тем же начальным числом и параметрами возвращали один и тот же результат. *Вроде можно создать как раз шаблонизирование с помощью этого* 
* **n** *optional, defaults = 1*
*  **user** *optional* - 
	Уникальный идентификатор. Отправка идентификаторов конечных пользователей в запросах может быть полезным инструментом, помогающим OpenAI отслеживать и обнаруживать злоупотребления. Это позволяет OpenAI предоставлять вашей команде более действенную обратную связь в случае обнаружения каких-либо нарушений политики в вашем приложении.  
	Идентификаторы должны представлять собой строку, которая однозначно идентифицирует каждого пользователя. Мы рекомендуем хешировать их имя пользователя или адрес электронной почты, чтобы избежать отправки нам какой-либо идентифицирующей информации. Если вы предлагаете предварительный просмотр своего продукта пользователям, не вошедшим в систему, вместо этого вы можете отправить идентификатор сеанса.

