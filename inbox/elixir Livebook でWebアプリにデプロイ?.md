---
type: note
---

#elixir 

---
2023-03-30  09:47

# elixir Livebook でWebアプリにデプロイ?

https://www.youtube.com/watch?v=8mXJI1FH3zQ

0.9らしい。

```shell
$ brew upgrade livebook

livebook 0.8.0 -> 0.9.0
```
## Livebook

```elixir
Mix.install([
	{:kino, "~> 0.9"},
])
```

```elixir
frame = Kino.Frame.new()

inputs = [
	name: Kino.Input.text("Name"),
	message: Kino.Input.text("Message")
]
form = Kino.Control.form(inputs, submit: "Send", reset_on_submit: [:message])
```
```elixir
Kino.listen(form, fn %{data: %{name: name, message: message}, origin: origin} ->
	if name != "" and message != "" do
		content = Kino.Markdown.new("**#{name}**: #{message}")
		Kino.Frame.append(frame, content)
	else
		content = Kino.Markdown.new("_ERROR! You need a name and message to submit..._")
		Kino.Frame.append(frame, content, to: origin)
	end
end)
```

デプロイはロケットアイコンで Slug を入れて Deploy 押すと?

![[Pasted image 20230330110106.png]]


localhost:0/apps/chat となって上手くいきませんでした。

