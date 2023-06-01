---
type: note
---

#elixir

---
2022-10-04  14:46

# elixir  Phoenix 入門

postgresql を brew で入れた。

```shell
$ brew install postgresql

$ psql --version
psql 14.5


$ brew services start postgresql


ログインする
$ psql postgres
ログアウトする

role で postgres が必要らしい。
$ createuser postgres
$ psql postgres
#\du
george
postgres

postgres に権限がないので、付与する。

# ALTER USER "postgres" WITH SUPERUSER
```

[[pop_os  PostgreSQL をいれる]]



Phenix のインストール

```shell
$ mix local.hex
$ mix archive.install hex phx_new

$ mix phx.new --version
Phoenix installer v1.6.13
```

Phoenix プロジェクトの作成  プロジェクト名 : realworld

```shell
$ mix phx.new realworld
$ cd realworld

$ git init
$ git add .
$ git commit -m 'first commit.'
```

### データベースの初期化 & 起動
```shell
$ mix ecto.setup
$ mix phx.server
```

![[Pasted image 20221004153632.png |400]]

ホットリロードしてる。


### スキャフォールド

```shell
$ mix phx.gen.live Blogs Article articles title:string body:text
```

### ルーティング
```ruby
# lib/realworld_web/router.ex

  scope "/", RealworldWeb do
    pipe_through :browser

    get "/", PageController, :index

    live "/articles", ArticleLive.Index, :index
    live "/articles/new", ArticleLive.Index, :new
    live "/articles/:id/edit", ArticleLive.Index, :edit

    live "/articles/:id", ArticleLive.Show, :show
    live "/articles/:id/show/edit", ArticleLive.Show, :edit
  end

```
### マイグレーション

```shell
$ mix ecto.migrate
```

http://localhost:4000/articles

![[Pasted image 20221004154904.png]]


### テスト
```shell
$ mix test
```

![[Pasted image 20221004155210.png]]

### データベース操作

```shell
$ iex -S mix
> alias Realworld.Blogs.Article
> alias Realworld.Repo
> import Ecto.Query, warn: false

作成
> %Article{} |> Article.changeset(%{title: "t1", body: "b1"}) |> Repo.insert()
> %Article{} |> Article.changeset(%{title: "t2", body: "b2"}) |> Repo.insert()


読み出し
> query = from a in Article
> Repo.all(query)

> (from a in query, where: a.title == 't1') |> Repo.all()

更新
> Repo.get(Article, 1) |> Article.changeset(%{body: "..."}) |> Repo.update()


削除
> Repo.delete_all(Article)
```

データリセット
```shell
$ mix ecto.reset
```

### コメントのリレーション

```shell
$  mix phx.gen.context Blogs Comment comments boyd:string article_id:references:articles
```

モデルコードの変更

```ruby
# lib/realworld/blogs/comment.ex


  alias Realworld.Blogs.Article

  schema "comments" do
    field :body, :string
    belongs_to :article_id, Article

    timestamps()
  end

  @doc false
  def changeset(comment, attrs) do
    comment
    |> cast(attrs, [:body, :article_id])
    |> validate_required([:body, :article_id])
  end
```

マイグレーションファイルの変更

```ruby
# lib/priv/repo/migrations/......._create_comments.exs
    create table(:comments) do
      add :body, :string, null: false
      add :article_id, references(:articles, on_delete: :nothing),null: false

      timestamps()
    end
```


記事のid が必須になったので、テストの変更

```ruby
# lib/test/realworld/blogs_test.exs


    test "create_comment/1 with valid data creates a comment" do
      valid_attrs = %{body: "some body"}

      assert {:ok, %Comment{} = comment, article_id: Map.get(article_fixture(), :id)} = Blogs.create_comment(valid_attrs)
      assert comment.body == "some body"
    end
```

```ruby
# lib/test/support/fixtures/blogs_fixtures.ex


  def comment_fixture(attrs \\ %{}) do
    {:ok, comment} =
      attrs
      |> Enum.into(%{
        body: "some body",
        article_id: Map.get(article_fixture(), :id)
      })
      |> Realworld.Blogs.create_comment()

    comment
  end
```



記事モデルの変更
```ruby
schema "articles" do

field :body, :string

field :title, :string

  

has_many :comments, Comment, on_delete: :delete_all

timestamps()

end
```

マイグレーション

```shell
$ mix ecto.migrate
```

### 記事のタグをつける

```shell
$ mix phx.gen.context Blogs Tag tags tag:string:unique
```

パースする関数
```ruby
# lib/realworld/blogs/tag.ex

  def parse(nil), do: parse("")

  def parse(tags) do
    tags
    |> String.split(",")
    |> Enum.map(&String.trim/1)
    |> Enum.reject(&(&1 == ""))
  end
```

中間テーブル作成

```shell
mix phx.gen.schema Blogs.ArticleTag articles_tags article_id:references:articles tag_id:references:tags
```

```ruby
# lib/realworld/blogs/article_tag.ex


  import Ecto.Changeset
  alias Realworld.Blogs.Article
  alias Realworld.Blogs.Tag


  @primary_key false
  schema "articles_tags" do

    belongs_to :article, Article
    belongs_to :tag, Tag

    timestamps()
  end

  @doc false
  def changeset(article_tag, attrs) do
    article_tag
    |> cast(attrs, [:tag_id, :article_id])
    |> validate_required([:tag_id, :article_id])
  end
```


```ruby
# lib/realworld/blogs/article.ex

  alias Realworld.Blogs.Comment
  alias Realworld.Blogs.Tag
  alias Realworld.Blogs.ArticleTag

  schema "articles" do
    field :body, :string
    field :title, :string

    has_many :comments, Comment, on_delete: :delete_all
    many_to_many :tags, Tag, join_through: ArticleTag, on_replace: :delete, on_delete: :delete_all
    timestamps()
  end

  @doc false
  def changeset(article, attrs, tags) do
    article
    |> cast(attrs, [:title, :body])
    |> validate_required([:title, :body])
    |> put_assoc(:tags, tags)
  end




  def list_articles do
    Repo.all(Article) |> Repo.preload(:tags)
  end

  def get_article!(id), do: Repo.get!(Article, id) |> Repo.preload(:tags)
```

```ruby
# lib/realworld/blogs.ex


  alias Realworld.Blogs.Tag


  def insert_article_with_tags(attrs) do
    insert_or_update_article_with_tags(%Article{}, attrs)
  end

  def insert_or_update_article_with_tags(article, attrs) do
    Ecto.Multi.new()
    |> Ecto.Multi.run(:tags, fn _repo, _changes ->
        insert_and_get_all_tags(attrs)
    end)
    |> Repo.transaction()
  end

  defp insert_and_get_all_tags(attrs) do
    case Tag.parse(attrs[:tags_string] || attrs["tags_string"]) do
      [] ->
        {:ok, []}
      names ->
        timestamp =
          NaiveDateTime.utc_now()
          |> NaiveDateTime.truncate(:second)
        maps =
          Enum.map(
            names,
            &%{
              tag: &1,
              inserted_at: timestamp,
              updated_at: timestamp
            }
          )
        Repo.insert_all(Tag, maps, on_conflict: :nothing)
        query = from t in Tag, where: t.tag in ^names
        {:ok, Repo.all(query)}
    end
  end

  defp insert_or_update_article(article, attrs, %{tags: tags}) do
    article
    |> Article.changeset(attrs, tags)
    |> Repo.insert_or_update()
  end

```


### タグによる記事検索

