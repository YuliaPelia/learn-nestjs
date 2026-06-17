# nest-learn

Навчальний проєкт на NestJS щоб виділити ключові моменти.

## Конспект: ProfilesController

Короткий огляд того, як працює `src/profiles/profiles.controller.ts`.

### Що це таке?

**Controller** — шар, який приймає HTTP-запити і повертає відповіді. Він не містить бізнес-логіки (поки що), а лише маршрутизує запити.

```ts
@Controller('profiles')
export class ProfilesController { ... }
```

`@Controller('profiles')` — усі маршрути починаються з `/profiles`.

### Ендпоінти

| Метод | URL | Декоратор | Що робить |
|-------|-----|-----------|-----------|
| `GET` | `/profiles` | `@Query()` | Читає query-параметри `?age=...&location=...` |
| `GET` | `/profiles/:id` | `@Param()` | Читає `id` з URL |
| `POST` | `/profiles` | `@Body()` | Читає JSON-тіло запиту |

### Ключові декоратори

- **`@Get()` / `@Post()`** — HTTP-метод і шлях (тут шлях порожній → базовий `/profiles`)
- **`@Query('age')`** — значення з URL: `/profiles?age=25&location=Kyiv`
- **`@Param('id')`** — значення з URL: `/profiles/abc123`
- **`@Body()`** — тіло запиту (JSON) перетворюється на об'єкт `CreateProfileDto`

### DTO (Data Transfer Object)

```ts
export class CreateProfileDto {
    name: string;
    description: string;
}
```

DTO описує **форму даних**, які очікує `POST`. Це контракт між клієнтом і сервером: які поля має містити JSON.

### Як працює `POST /profiles`

1. Клієнт надсилає JSON:

```json
{
  "name": "Kai",
  "description": "This JavaScript coder"
}
```

2. Nest парсить тіло → `createProfileDto`
3. Контролер повертає ті самі поля у відповіді

**Важливо:** для парсингу JSON потрібен заголовок `Content-Type: application/json`. Без нього `@Body()` буде порожнім або `undefined`, і виникне помилка.

### Структура модуля

```
profiles/
├── profiles.controller.ts   ← маршрути (HTTP)
├── profiles.module.ts       ← реєструє контролер
└── dto/
    └── create-profile.dto.ts  ← форма даних для POST
```

`ProfilesModule` підключений у `AppModule` — тоді Nest знає про ці маршрути.

### Що варто запам'ятати

1. **Controller = маршрути**, не логіка зберігання даних
2. **`@Query`** — параметри в URL після `?`
3. **`@Param`** — частина шляху (`:id`)
4. **`@Body`** — JSON у тілі запиту
5. **DTO** — опис структури вхідних даних
6. **POST потребує `Content-Type: application/json`**

### Типовий наступний крок у NestJS

- `ProfilesService` — бізнес-логіка
- `ValidationPipe` + `class-validator` — перевірка полів (у `main.ts` вже підключено)
- База даних — збереження профілів
