---
type: note
---

#ruby/rails 

---
2022-05-18  16:34

# rails  ページネーション kaminari のデザインカスタマイズ

## config/initializers

kaminari の設定ファイルを生成する
```shell
$ rails g kaminari:config
```

1page につき 10件にする時。

config/initializers/kaminari_config.rb

```ruby
Kaminari.configure do |config|
  config.default_per_page = 10
```



## controllers/
app/controllers/cats_controller.rb

```ruby
def index
    @search = Cat.ransack(params[:q])
    @search.sorts = 'id desc' if @search.sorts.empty?
    @cats = @search.result.page(params[:page])
end
```

## views/cats/

app/views/cats/index.html.erb

```html
    <div class="d-flex justify-content-end mt-3">
      <%= paginate @cats %>
    </div>
```


## views/kaminari/

`app/views/kaminari/_first_page.html.erb`

```html
<li class="page-item">
  <%= link_to_unless current_page.first?, raw(t 'views.pagination.first'), url,
      remote: remote, class: 'page-link' %>
</li>
```

`app/views/kaminari/_gap.html.erb`

```html
<li class="page-item disabled">
  <%= link_to raw(t 'views.pagination.truncate'), '#', class: 'page-link' %>
</li>
```

`app/views/kaminari/_last_page.html.erb`

```html
<li class="page-item">
  <%= link_to_unless current_page.last?, raw(t 'views.pagination.last'), url,
    remote: remote, class: 'page-link' %>
</li>
```

`app/views/kaminari/_next_page.html.erb`

```html
<li class="page-item">
  <%= link_to_unless current_page.last?, raw(t 'views.pagination.next'), url,
    rel: 'next', remote: remote, class: 'page-link' %>
</li>
```

`app/views/kaminari/_page.html.erb`

```html
<% if page.current? %>
  <li class="page-item active">
    <%= content_tag :a, page, data: { remote: remote }, rel: page.rel, class: 'page-link' %>
  </li>
<% else %>
  <li class="page-item">
    <%= link_to page, url, remote: remote, rel: page.rel, class: 'page-link' %>
  </li>
<% end %>
```

`app/views/kaminari/_pagenator.html.erb`

```html
<%= paginator.render do%>
  <nav>
    <ul class="pagination">
      <%= first_page_tag unless current_page.first? %>
      <%= prev_page_tag unless current_page.first? %>
      <% each_page do |page| %>
        <% if page.left_outer? || page.right_outer? || page.inside_window? %>
          <%= page_tag page %>
        <% elsif !page.was_truncated? -%>
          <%= gap_tag %>
        <% end %>
      <% end %>
      <%= next_page_tag unless current_page.last? %>
      <%= last_page_tag unless current_page.last? %>
    </ul>
  </nav>
<% end %>
```

`app/views/kaminari/_prev_page.html.erb`

```html
<li class="page-item">
  <%= link_to_unless current_page.first?, raw(t 'views.pagination.previous'), url,
      rel: 'prev', remote: remote, class: 'page-link' %>
</li>
```


![[Pasted image 20220518163527.png]]

