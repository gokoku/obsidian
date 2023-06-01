#js

---
2021-11-17

# Nest.js 入門 1 日目

アベントカレンダー

https://qiita.com/potato4d/items/aabb78fd201592352d64


```shell
$ npx -p @nestjs/cli nest new nest-project-day1
$ cd next-project-day1
$ npm run start
```

http://localhost:3000

![[Pasted image 20211117100151.png]]

* app.service.ts 
* app.controller.ts
* app.module.ts

`@Injectable` を定義した Service は、 Module の Provider として定義されることによって、 Controller への依存オブジェクトの注入を実行します。

一番汎用的な枠が Provider となります。

app.service.ts

```ts
import { Injectable } from '@nestjs/common';

export interface Item {
  id: number;
  title: string;
  body: string;
  deletePassword: string;
}

export type PublicItem = Omit<Item, 'deletePassword'>;
const items: Item[] = [
  {
    id: 1,
    title: "Item title",
    body: "Hello World",
    deletePassword: "12345",
  },
];

@Injectable()
export class AppService {

  getAllItems(): Item[] {
    return [...items];
  }

  getPublicItem(): PublicItem[] {
    return this.getAllItems().map(item => {
      const publicItem = {...item};
      delete publicItem.deletePassword;
      return publicItem;
    });
  }
}
```

app.controller.ts

```ts
import { Controller, Get } from '@nestjs/common';
import { AppService, PublicItem } from './app.service';

@Controller()
export class AppController {
  constructor(private readonly appService: AppService) {}

  @Get('items')
  getItems(): PublicItem[]{
    return this.appService.getPublicItem();
  }
}

```
### テスト

controller から service をモックにして切り離した。

```ts
import { Test, TestingModule } from '@nestjs/testing';
import { AppController } from './app.controller';
import { AppService, PublicItem } from './app.service';

describe('AppController', () => {
  let appController: AppController;
  let appService: AppService;

  beforeEach(async () => {
    appService = new AppService();
    appController = new AppController(appService);
  });

  describe('/items', () => {
    it('should return public items', () => {
      jest.spyOn(appService, 'getPublicItem').mockImplementation(() => {
        const item: PublicItem = {
          id: 1,
          title: 'Mock Title',
          body: 'Mock Body',
        };
        return [item];
      });
      expect(appController.getItems()).toHaveLength(1);
    });
  });
});

```

切り離した service のために unit テスト。

```ts
import {Test, TestingModule} from '@nestjs/testing';
import {AppService} from './app.service';

describe('AppService', () => {
  let service: AppService;

  beforeEach(async () => {
    const module: TestingModule = await Test.createTestingModule({
      providers: [AppService],
    }).compile();

    service = module.get<AppService>(AppService);
  });

  it('should be defined', () => {
    expect(service).toBeDefined();
  });

  describe('getPublicItems', () => {
    it('should have deletePassword removed', () => {
      const publicItems = service.getPublicItem();
      expect(publicItems.every((item) => !('deletePasswlrd' in item))).toBe(true);
    });
  });
});
```
