---
type: note
---

#ruby/rails 

---
2022-08-25  11:42

# rails. 多対多リレーションリブート

こんがらかってたので、再入門した。

https://programming.mmsankosho.com/archives/17718?_ga=2.25149437.693853629.1661386622-954632818.1661386622


![[Pasted image 20220825115100.png]]

講師が講義を持っていて、生徒は講義にアサインする形。

```shell
$ rails new tataita
$ cd tataita


$ rails g model lecturer name:string
$ rails g model student name:string
$ rails g model lecture time:string lecturer:references
$ rails g model model appointment lecture:references student:references

$ rails db:migrate
```

### rails model

```ruby
app/models/lecturer.rb

class Lecturer < ApplicationRecord
  has_many :lectures
end

---------------------------------
app/models/recture.rb

class Lecture < ApplicationRecord
  belongs_to :lecturer
  has_many :appointments, dependent: :destroy
  has_many :students, through: :appointments
end

---------------------------------

app/models/student.rb

class Student < ApplicationRecord
  has_many :appointments, dependent: :destroy
  has_many :lectures, through: :appointments
end


---------------------------------

app/models/appointment.rb

class Appointment < ApplicationRecord
  belongs_to :lecture
  belongs_to :student
end
```

```shell
$ rails c

講師追加

> @lecturer1 = Lecturer.create name: "中田"
> @lecturer2 = Lecturer.create name: "佐々木"

レクチャー追加

> @lecture1 = @lecturer1.lectures.create(time: "10:00")
> @recture2 = @lecturer2.lectures.create(time: "15:00")


生徒追加

> @student1 = Student.create name: "秋山"
> @student2 = Student.create name: "野口"

講義の予約を入れる

> @student1.lectures << @lecturer1
> @student1.lectures << @lecturer2


----------------
@student1 の予約した講義を見る

> @student1.lectures

最初の講義の講師を見る

> @student1.lectures.first.lecturer
```

