<p align="center">
  <a href="https://t.me/codyapi"><img src="https://img.shields.io/badge/Cody%20API-2684FF?style=for-the-badge&logo=telegram&logoColor=white" alt="Cody API" /></a>
</p>

<!-- Language Switch -->
<p align="right">
  <a href="#english">🇬🇧 English</a> | <a href="#русский">🇷🇺 Русский</a>
</p>

<a id="english"></a>
Here is the updated English version of the documentation, aligned with the content of the provided Russian text:

# Cody API

**Free, unlimited access to modern language and multimodal models via an OpenAI-compatible endpoint.**

### Contents
- [Key Features](#key-features)
- [1. Get an API Key](#1-get-an-api-key)
- [2. Quick Start](#2-quick-start-python--openai-sdk)
- [3. Endpoints](#3-endpoints)
- [4. Models](#4-models)
- [5. Limits](#5-limits)
- [6. Security & Privacy](#6-security--privacy)
- [7. Support](#7-support)

## Key Features
- ⚡ **Drop-in replacement for the OpenAI SDK** — just change two lines of code.
- 🆓 **Completely free**, no hard quotas.
- 🔒 **Zero-retention** architecture: request content is not saved.
- 📷 **Multimodal**: text, image generation/editing, TTS.
- 🚀 Catalog of **25+ current SOTA models**.

---

## 1. Get an API Key
Contact the API administrator on Telegram.

## 2. Quick Start (Python + OpenAI SDK)

### Text
```python
from openai import OpenAI

client = OpenAI(base_url="https://codyapi.ru/v1", api_key="cody-...")

completion = client.chat.completions.create(
    model="gpt-5.2",
    messages=[{"role": "user", "content": "A short story about a kitten"}],
)
print(completion.choices[0].message.content)
```

### Streaming
```python
from openai import OpenAI

client = OpenAI(base_url="https://codyapi.ru/v1", api_key="cody-...")

completion = client.chat.completions.create(
     model="gpt-5.2",
    messages=[{"role": "user", "content": "Hi, write a story about 2 cats: Sonya and Alice"}],
    stream=True,
)

for chunk in completion:
    if chunk.choices and chunk.choices[0].delta.content:
        print(chunk.choices[0].delta.content, end="", flush=True)
```

### Image
```python
from openai import OpenAI
import base64, pathlib

client = OpenAI(base_url="https://codyapi.ru/v1", api_key="cody-...")

img = client.images.generate(
    model="gpt-image-1.5",
    prompt="A veterinarian listening to a baby otter’s heartbeat, children’s book style",
)

pathlib.Path("otter.png").write_bytes(base64.b64decode(img.data[0].b64_json))
```

### Image Edit
```python
from openai import OpenAI
import base64, pathlib

client = OpenAI(base_url="https://codyapi.ru/v1", api_key="cody-...")

edited = client.images.edit(
    model="gpt-image-1.5",
    prompt="Add sunglasses",
    image=[open("otter.png", "rb")]
)

pathlib.Path("otter_edit.png").write_bytes(base64.b64decode(edited.data[0].b64_json))
```
> **Note:** The `images.generate` and `images.edit` endpoints return images **only** in Base64 (`b64_json`).

### Supported Provider Groups
In cody-api, you can specify `group` in `extra_body`; this determines which group of providers your request will be sent to.

Example of calling a specific group:
```python
from openai import OpenAI

client = OpenAI(base_url="https://codyapi.ru/v1", api_key="cody-...")

completion = client.chat.completions.create(
    model="claude-opus-4.5",
    messages=[{"role": "user", "content": "Hello"}],
    extra_body={
        "group": "openrouter"
    }
)
print(completion.choices[0].message.content)
```

### Supported Provider Parameters
You can use any parameters supported by the providers; requests must be sent via `chat.completions`.

For the `openai` provider, you can send requests to any supported endpoints.

Example of working with the `fal-ai` provider:
```python
from openai import OpenAI

client = OpenAI(base_url="https://codyapi.ru/v1", api_key="cody-...")

completion = client.chat.completions.create(
    model="sora-2/text-to-video",
    messages=[{"role": "user", "content": "cat"}],
    extra_body={
        "group": "fal-ai",
        "duration": 8
    }
)

print(completion.choices[0].message.content)
```

### Getting request_id
If you specify `previous_request_id` in `extra_body` of a subsequent request, that request will be routed to the same provider as the request with that `request_id`.

```python
from openai import OpenAI

client = OpenAI(base_url="https://codyapi.ru/v1", api_key="cody-...")

completion = client.chat.completions.with_raw_response.create(
    model="claude-opus-4.5",
    messages=[{"role": "user", "content": "Hello"}],
    extra_body={
        "group": "openrouter"
    }
)

request_id = completion.headers.get("x-request-id")
print(request_id)

completion = completion.parse()
print(completion.choices[0].message.content)
```

---

## 3. Endpoints

| Endpoint | Status |
|----------|--------|
| `chat.completions` | ✅ |
| `responses` | ✅ |
| `images.generate`  | ✅ |
| `images.edit`      | ✅ |

## 4. Models
```python
from openai import OpenAI
import base64, pathlib

client = OpenAI(base_url="https://codyapi.ru/v1", api_key="cody-...")

for model in client.models.list().data:
    print("id:", model.id)
    print("cost:", model.cost)
    print("available_groups:", model.available_groups)
    print()
```

#### Supported Models  
See the full catalog of supported models in [MODELS.md](./MODELS.md).


## 5. Limits
- 40 requests per minute
- 20 requests per second
- Credit limits are assigned differently for each client.
Updates are published in the [Telegram channel](https://t.me/codyapi).

## 6. Security & Privacy
- Zero-retention: data is stored only in RAM.
- Requests/responses are not logged.

## 7. Support
- **Email:** vvirtr@gmail.com
- **Telegram:** [@vvirtr](https://t.me/vvirtr)
- **Channel:** [t.me/codyapi](https://t.me/codyapi)

---

<a id="русский"></a>
# Cody API

**Бесплатный безлимитный доступ к современным языковым и мультимодальным моделям через совместимый с OpenAI endpoint.**

### Оглавление
- [Ключевые возможности](#ключевые-возможности)
- [1. Получить API-ключ](#1-получить-api-ключ)
- [2. Быстрый старт](#2-быстрый-старт-python--openai-sdk)
- [3. Эндпоинты](#3-эндпоинты)
- [4. Модели](#4-модели)
- [5. Ограничения](#5-ограничения)
- [6. Безопасность и конфиденциальность](#6-безопасность-и-конфиденциальность)
- [7. Поддержка](#7-поддержка)

## Ключевые возможности
- ⚡ **Drop-in замена OpenAI SDK** — достаточно изменить две строки кода.
- 🆓 **Полностью бесплатно**, без жёстких квот.
- 🔒 **Zero-retention** архитектура: содержимое запросов не сохраняется.
- 📷 **Мультимодальность**: текст, генерация/редактирование изображений, TTS.
- 🚀 Каталог из **25+ актуальных SOTA моделей**.

---

## 1. Получить API-ключ
Напишите администратору api в telegram

## 2. Быстрый старт (Python + OpenAI SDK)

### Текст
```python
from openai import OpenAI

client = OpenAI(base_url="https://codyapi.ru/v1", api_key="cody-...")

completion = client.chat.completions.create(
    model="gpt-5.2",
    messages=[{"role": "user", "content": "Короткая история про котёнка"}],
)
print(completion.choices[0].message.content)
```

### Стриминг
```python
from openai import OpenAI

client = OpenAI(base_url="https://codyapi.ru/v1", api_key="cody-...")

completion = client.chat.completions.create(
     model="gpt-5.2",
    messages=[{"role": "user", "content": "Привет, напиши историю про 2х кошек: Соню и Алису"}],
    stream=True,
)

for chunk in completion:
    if chunk.choices and chunk.choices[0].delta.content:
        print(chunk.choices[0].delta.content, end="", flush=True)
```

### Изображение
```python
from openai import OpenAI
import base64, pathlib

client = OpenAI(base_url="https://codyapi.ru/v1", api_key="cody-...")

img = client.images.generate(
    model="gpt-image-1.5",
    prompt="Ветеринар слушает сердце детёныша выдры, стиль детской книги",
)

pathlib.Path("otter.png").write_bytes(base64.b64decode(img.data[0].b64_json))
```

### Редактирование изображения
```python
from openai import OpenAI
import base64, pathlib

client = OpenAI(base_url="https://codyapi.ru/v1", api_key="cody-...")

edited = client.images.edit(
    model="gpt-image-1.5",
    prompt="Add sunglasses",
    image=[open("otter.png", "rb")]
)

pathlib.Path("otter_edit.png").write_bytes(base64.b64decode(edited.data[0].b64_json))
```
> **Примечание:** эндпоинты `images.generate` и `images.edit` возвращают изображения **только** в Base64 (`b64_json`).

### Поддерживаемые группы провайдеров
В cody-api вы можете указать group в extra_body, от этого зависит, к какой группе провайдеров отошлется ваш запрос.

Пример вызова определенной группы:
```python
from openai import OpenAI

client = OpenAI(base_url="https://codyapi.ru/v1", api_key="cody-...")

completion = client.chat.completions.create(
    model="claude-opus-4.5",
    messages=[{"role": "user", "content": "Привет"}],
    extra_body={
        "group": "openrouter"
    }
)
print(completion.choices[0].message.content)
```

### Поддерживаемые параметры провайдеров
Вы можете использовать любые поддерживаемые параметры провайдеров, отправлять запрос нужно по chat.completions.

В провайдере openai можно слать запросы на любые поддерживаемые endpointы.

Пример работы с провайдером fal-ai:
```python
from openai import OpenAI

client = OpenAI(base_url="https://codyapi.ru/v1", api_key="cody-...")

completion = client.chat.completions.create(
    model="sora-2/text-to-video",
    messages=[{"role": "user", "content": "кот"}],
    extra_body={
        "group": "fal-ai",
        "duration": 8
    }
)

print(completion.choices[0].message.content)
```

### Получение request_id
Если указать в следующем запросе previous_request_id в extra_body, то запрос пойдет к тому-же провайдеру, что и запрос с request_id.
```python
from openai import OpenAI

client = OpenAI(base_url="https://codyapi.ru/v1", api_key="cody-...")

completion = client.chat.completions.with_raw_response.create(
    model="claude-opus-4.5",
    messages=[{"role": "user", "content": "Привет"}],
    extra_body={
        "group": "openrouter"
    }
)

request_id = completion.headers.get("x-request-id")
print(request_id)

completion = completion.parse()
print(completion.choices[0].message.content)
```

---

## 3. Эндпоинты

| Endpoint | Статус |
|----------|--------|
| `chat.completions` | ✅ |
| `responses` | ✅ |
| `images.generate`  | ✅ |
| `images.edit`      | ✅ |

## 4. Модели
```python
from openai import OpenAI
import base64, pathlib

client = OpenAI(base_url="https://codyapi.ru/v1", api_key="cody-...")

for model in client.models.list().data:
    print("id:", model.id)
    print("cost:", model.cost)
    print("available_groups:", model.available_groups)
    print()
```

#### Поддерживаемые модели  
Полный каталог поддерживаемых моделей см. в [MODELS.md](./MODELS.md).


## 5. Ограничения
- 40 запросов в минуту
- 20 запросов в секунду
- Лимиты по кредитам, выдаются каждому клиенту по-разному
Обновления публикуются в [Telegram-канале](https://t.me/codyapi).

## 6. Безопасность и конфиденциальность
- Zero-retention: данные хранятся только в RAM.
- Запросы/ответы не логируются.

## 7. Поддержка
- **Email:** vvirtr@gmail.com
- **Telegram:** [@vvirtr](https://t.me/vvirtr)
- **Канал:** [t.me/codyapi](https://t.me/codyapi)
