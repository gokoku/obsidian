---
type: note
---

#ruby/rails 

---
2023-03-07  17:00

# rails タスクを作る

```shell
$ rails g task set_gtfs
```

lib/tasks/set_gtfs.rake
```ruby
namespace :set_gtfs do
  desc "GTFSファイルを読み込んでDBにセットする"
  task :read_gtfs do
    puts "hello world"
  end
end
```

```shell
# 実行
$ rails set_gtfs:read_gtfs
hello world

# help
$ rails -vT
```

![[Pasted image 20230307170928.png]]


## タスクの中の別タスク

```ruby
namespace :tasks do
  task :task_1, [:word] do |_, args|
    puts "Hello, #{args.word}"
  end

  task :task_2 do
    Rake::Task['tasks:task_1'].invoke('Rails')
  end
end
```

```shell
$ rails tasks:task_2
Hello, Rails
```

