#js

---
2021-11-17

# js Nest.js 入門2日目

https://qiita.com/potato4d/items/d22a14ff6fb82d63c742

NestJS での DTO とは、Request Oject みたいな意味らしい。

-   NestJS の DTO は Request Payload(body) の型定義を行うためのもの
-   一般的な DAO / DTO の文脈とは少し違った印象を受けるイメージ
-   **型定義と同時にバリデーションまで含む**ことができる

つまり、DTO レイヤーを挟むと、controller に渡ってくるデータはバリデーション済みになるとのこと。


```shell
$ npx -p @nestjs/cli nest new day3-dto-and-validation
$ cd day3-dto-and-validation
$ npm run start:dev
```

DTO を使えるようにする。

```shell
$ yarn add class-transformer class-validator
```


items.dto.ts

```ts
import { IsNotEmpty, IsString } from "class-validator";

export class CreateItemDTO {

  @IsNotEmpty()
  @IsString()
  title: string;

  @IsNotEmpty()
  @IsString()
  body: string;

  @IsNotEmpty()
  @IsString()
  deletePassword: string;
}
```

items.controller.ts

```ts
import {Controller, Post, Body} from '@nestjs/common'
import { CreateItemDTO } from './items.dto';

@Controller('items')
export class ItemsController {
  @Post()
  createItem(@Body() createItemDTO: CreateItemDTO) {
    return true;
  }
}
```

アクセス出来るようにまとめる。

app.module.ts

```ts
import { Module } from '@nestjs/common';
import { AppController } from './app.controller';
import { AppService } from './app.service';
import { ItemsController } from './items.controller';

@Module({
  imports: [],
  controllers: [AppController, ItemsController],
  providers: [AppService],
})
export class AppModule {}

```


curl で POST する。
```shell
バリデーション失敗

$ curl -X POST -d 'test="test"' http://localhost:3000/items

{
  "statusCode": 400,
  "message": [
    "body must be a string",
    "body should not be empty",
    "deletePassword must be a string",
    "deletePassword should not be empty"
  ],
  "error": "Bad Request"
}


バリデーション成功

$ curl -v -X POST -d 'title="test"' -d 'body="test"' -d 'deletePassword="test"' http://localhost:3000/items | jq

true
```





