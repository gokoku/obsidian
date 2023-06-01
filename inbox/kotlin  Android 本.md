#kotlin #android

---
2021-10-12

# Android 本

Mastodon client を作る本。

Mastodon の JSON こんな感じ。

```json

[
 {"id":"107059434774398235",
  "created_at":"2021-10-07T09:02:12.453Z",
  "in_reply_to_id":null,
  "in_reply_to_account_id":null,
  "sensitive":false,
  "spoiler_text":"",
  "visibility":"public",
  "language":"en",
  "uri":"https://androidbook2020.keiji.io/users/muradhassan/statuses/107059434774398235",
  "url":"https://androidbook2020.keiji.io/@muradhassan/107059434774398235",
  "replies_count":0,
  "reblogs_count":0,
  "favourites_count":0,
  "content":"\u003cp\u003eSWEDISH artist, who survived two murder attempts for his cartoons of the Prophet Mohammed, 
       was killed in a suspicious car crash along with his two police bodyguards\u003cbr /\u003eLars Vilks, 75,
       survived a series of assassination attempts\u003cbr /\u003e\u003ca href=\"https://barenakedislam.com/2021
       /10/04/swedish-artist-who-survived-two-murder-attempts-for-his-cartoons-of-the-prophet-mohammed-killed-in-
       a-suspicious-car-crash-along-with-his-two-police-bodyguards/\" rel=\"nofollow noopener noreferrer\" 
       target=\"_blank\"\u003e\u003cspan class=\"invisible\"\u003ehttps://\u003c/span\u003e\u003cspan 
       class=\"ellipsis\"\u003ebarenakedislam.com/2021/10/04/\u003c/span\u003e\u003cspan 
       class=\"invisible\"\u003eswedish-artist-who-survived-two-murder-attempts-for-his-cartoons-of-the-prophet-mohammed-
       killed-in-a-suspicious-car-crash-along-with-his-two-police-bodyguards/\u003c/span\u003e\u003c/a\u003e\u003c/p\u003e",
  "reblog":null,
  "application":{
  "name":"Web",
  "website":null},

  "account":{
  "id":"301",
  "username":"muradhassan",
  "acct":"muradhassan",
  "display_name":"",
  "locked":false,
  "bot":false,
  "discoverable":false,
  "group":false,
  "created_at":"2021-10-07T08:49:56.994Z",
  "note":"\u003cp\u003eChristians Are The World’s Most Oppressed Religious Group\u003cbr /\u003eChristians continue to be th
      e world\u0026apos;s most oppressed religious group, with persecution against them reported in 110 countries\u003cbr /\u
      003e\u003ca href=\"https://www.cnsnews.com/news/article/barbara-boland/pew-study-christians-are-world-s-most-oppressed-
      religious-group\" rel=\"nofollow noopener noreferrer\" target=\"_blank\"\u003e\u003cspan class=\"invisible\"\u003ehttps:
      //www.\u003c/span\u003e\u003cspan class=\"ellipsis\"\u003ecnsnews.com/news/article/barba\u003c/span\u003e\u003cspan cla
      ss=\"invisible\"\u003era-boland/pew-study-christians-are-world-s-most-oppressed-religious-group\u003c/span\u003e\u003c/ 
      a\u003e\u003c/p\u003e","url":"https://androidbook2020.keiji.io/@muradhassan","avatar":"https://androidbook2020.keiji.io
      /system/accounts/avatars/000/000/301/original/36d131ea491bae68.jpg?1633597112","avatar_static":"https://androidbook2020
      .keiji.io/system/accounts/avatars/000/000/301/original/36d131ea491bae68.jpg?1633597112","header":"https://androidbook20
      20.keiji.io/system/accounts/headers/000/000/301/original/c73b4602f65e2f6b.jpg?1633597112",
      "header_static":"https://androidbook2020.keiji.io/system/accounts/headers/000/000/301/original/c73b4602f65e2f6b.jpg?1633597112",
      "followers_count":0,
      "following_count":1,
      "statuses_count":3,
      "last_status_at":"2021-10-07",
      "emojis":[],
      "fields":[]},
      "media_attachments":[],
      "mentions":[],
      "tags":[],
      "emojis":[],
      "card":{
        "url":"https://barenakedislam.com/2021/10/04/swedish-artist-who-survived-two-murder-attempts-for-his-cartoons-of-the-prop
           het-mohammed-killed-in-a-suspicious-car-crash-along-with-his-two-police-bodyguards/","title":"SWEDISH artist, who survi
           ved two murder attempts for his cartoons of the Prophet Mohammed, was killed in a suspicious car crash along with his t
           wo police bodyguards",
         "description":"Lars Vilks, 75, who survived a series of assassination attempts and has had to live with 24/7 police prot
           ection since he drew controversial cartoons of the Prophet Mohammed in 2007 which set off Mu…",
         "type":"link",
         "author_name":"BareNakedIslam",
         "author_url":"https://barenakedislam.com/author/admin/",
         "provider_name":"BARE NAKED ISLAM",
         "provider_url":"https://barenakedislam.com",
         "html":"",
         "width":400,
         "height":171,
         "image":"https://androidbook2020.keiji.io/system/preview_cards/images/000/000/053/original/d77038a03fef13f9.png?1633597334",
         "embed_url":""
       },
       "poll":null
       },

```

