---
type: note
---

#elixir 

---
2023-01-11  19:52

# elixir Phoenix の触り

```shell
$ mix archive.install hex phx_new

$ mix phx.new basic --no-ecto
```

```shell
$ iex -S mix phx.server
ここにサーバログが出るし、repl になってる。
iex>
```

## DB Mnesia
分散DB ??

mix.exs
```elixir
def application do
 [
	 mod: {Basic.Application, []},
	 extra_applications: [:logger, :runtime_tools, :mnesia]
 ]
```
### Mnesia 用のスキーマを作成。

```shell
iex> :mnesia.create_schema([node()])
iex> :mnesia.start
iex> ls

Mnesia.nonode@nohost <--このフォルダにデータがほぞんされるとの事。

iex> :mnesia.create_table(:members, [attributes: [:id, :name, :age, :team, :position], disc_copies: [node()]])


失敗したらデリート
iex> :mnesia.delete_table(:members)
```

###  データ投入

lib/basic.ex
```elixir
defmodule Basic do

	def inserts() do

		:mnesia.dirty_write({:members, 1, "enぺだーし", 54, "有限会社有限会社でらいとシステムズ", "代表取締役、性能探求者"})
		:mnesia.dirty_write({:members, 2, "ざっきー", 50, "公立大学法人　北九州市立大学", "准教授、カーネルハッカー"})
		:mnesia.dirty_write({:members, 3, "たくと", 40, "カラビナテクノロジー株式会社", "リードエンジニア"})
		:mnesia.dirty_write({:members, 4, "piacere", 48, "株式会社DigDockConsulting", "常務取締役CTO、技術顧問、重力プログラマ"})
	end
end
```

このコードをコンパイルして実行。
```shell
iex> recompile
iex> Basic.inserts

投入したデータを確認
iex> :ets.i(:members)

日本語がバイナリなので
iex> :mnesia.transaction(fn -> :mnesia.read(:members, 1) end)

iex> :mnesia.dirty_read(:members, 1)
```

lib/util/db_mnesia.ex
```elixir
defmodule DbMnesia do
	def select(table_name) do
		:mnesia.start
		table_atom = String.to_atom(table_name)
		:mnesia.wait_for_tables([table_atom], 1000)
		columns = :mnesia.table_info(table_atom, :attributes) |> Enum.map(& Atom.to_string(&1))
		specs = [table_atom] ++ Enum.map(1..Enum.count(columns), & :"$#{&1}") |> List.to_tuple
		rows = :mnesia.transaction(fn ->
			:mnesia.select(table_atom, [{specs, [], [:"$$"]}]) end) |> elem(1)
		%{
			columns: columns,
			command: :select,
			connection_id: 0,
			num_rows: Enum.count(rows),
			rows: rows
		}
	end
end
```

lib/util/db.ex
```elixir
defmodule Db do
	def query(sql) when sql != "" do
		cond do
			String.match?(sql, ~r/select.*/) -> select(sql)
			String.match?(sql, ~r/insert.*/) -> {:error, "insert unsupported"}
			String.match?(sql, ~r/update.*/) -> {:error, "update unsupported"}
			String.match?(sql, ~r/delete.*/) -> {:error, "delete unsupported"}
		end
	end
	def select(sql) do
		Regex.named_captures(~r/select( *)(?<columns>.*)( *)from( *)(?<tables>.*)/, sql)["tables"]
		|> DbMnesia.select
	end
	def columns_rows(result) do
		result
		|> rows
		|> Enum.map(fn row -> Enum.into(List.zip([columns(result), row]), %{}) end)
	end
	def rows(%{rows: rows} = _result), do: rows
	def columns(%{columns: columns} = _result), do: columns
end
```


lib/basic_web/templates/page/index.html.heex
```
<section class="phx-hero">
	<h1><%= gettext "Welcome to %{name}!", name: "Phoenix" %></h1>
	<p>Peace of mind from prototype to production</p>
</section>

 <%
	datas = Db.query("select * from members") |> Db.columns_rows
	%>
<table>
	<%= for n <- datas do %>
		<tr>
			<td><%= n["name"] %></td>
			<td><%= n["age"] %></td>
			<td><%= n["team"] %></td>
			<td><%= n["position"] %></td>
		</tr>
	<% end %>
</table>
```




